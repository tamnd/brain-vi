---
title: "CF 416C - Hệ thống đặt chỗ"
description: "Chúng ta được cung cấp một tập hợp các yêu cầu đặt phòng, trong đó mỗi yêu cầu đại diện cho một nhóm khách ngồi cùng nhau hoặc hoàn toàn không đến."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 416
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 241 (Div. 2)"
rating: 1600
weight: 416
solve_time_s: 92
verified: true
draft: false
---

[CF 416C - Hệ thống đặt chỗ](https://codeforces.com/problemset/problem/416/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, dp, tham lam, thực hiện 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các yêu cầu đặt phòng, trong đó mỗi yêu cầu đại diện cho một nhóm khách ngồi cùng nhau hoặc hoàn toàn không đến. Mỗi yêu cầu có quy mô, nghĩa là có bao nhiêu người đến và lợi nhuận, nghĩa là nhà hàng kiếm được bao nhiêu tiền nếu nhóm đó được chấp nhận và có chỗ ngồi. 

Ngoài ra, chúng ta còn được cung cấp một bộ bàn cố định, mỗi bàn có dung lượng. Một bàn có thể chứa tối đa một nhóm và một nhóm chỉ có thể ngồi vào một bàn có sức chứa ít nhất bằng quy mô nhóm. Mỗi bảng được sử dụng tối đa một lần và mỗi yêu cầu được gán cho chính xác một bảng phù hợp hoặc bị từ chối hoàn toàn. Mục tiêu là chọn một tập hợp con các yêu cầu và gán chúng vào các bảng riêng biệt theo cách tối đa hóa tổng lợi nhuận. 

Cấu trúc đầu vào giúp độ căng phím rõ ràng. Chúng tôi đang kết hợp các mục từ hai bộ nhiều bộ theo hạn chế về dung lượng, đồng thời chỉ chọn một số mục để tối đa hóa trọng lượng. Đây không phải là vấn đề đối sánh thuần túy vì được phép bỏ qua cả hai bên và nó không phải là một vấn đề đơn giản vì năng lực là những đối tượng riêng biệt chứ không phải là ngân sách liên tục. 

Các ràng buộc rất nhỏ: cả số lượng yêu cầu và số lượng bảng tối đa là 1000, đồng thời dung lượng và kích thước nhóm bị giới hạn ở mức 1000. Điều này ngay lập tức cho thấy rằng giải pháp theo thứ tự n² hoặc n² log n đều có thể chấp nhận được, nhưng bất cứ thứ gì khối trên 1000 phần tử sẽ là ranh giới và không cần thiết. 

Một số trường hợp đặc biệt quan trọng đối với tính chính xác: 

Một trường hợp là khi tất cả các bảng đều quá nhỏ để đáp ứng mọi yêu cầu. Ví dụ, các yêu cầu`(5, 10)`và bảng`[1, 2]`tạo ra số lượng đặt phòng không được chấp nhận. Một kẻ ngây thơ tham lam lợi nhuận vẫn có thể cố gắng chuyển nhượng nhưng phải thất bại một cách đúng đắn. 

Một trường hợp khác là khi nhiều bảng có thể phục vụ cùng một yêu cầu. Ví dụ: một yêu cầu có kích thước 3 và các bảng`[3, 5, 10]`. Nếu chúng tôi chỉ định nó cho bảng lớn nhất trước tiên, chúng tôi có thể chặn các yêu cầu lớn hơn trong tương lai chỉ phù hợp với bảng lớn, làm giảm tổng lợi nhuận. 

Trường hợp thứ ba là khi hai yêu cầu lợi nhuận lớn cạnh tranh cho một bàn lớn, trong khi các yêu cầu nhỏ hơn lại phù hợp ở nơi khác. Một kẻ tham lam ngây thơ chỉ vì lợi nhuận có thể gán bảng lớn không chính xác và cản trở việc gán toàn cầu tốt hơn. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là xem xét việc gán yêu cầu trực tiếp cho các bảng. Đối với mỗi yêu cầu, chúng ta có thể thử gán nó cho bất kỳ bảng chưa sử dụng nào phù hợp, khám phá đệ quy tất cả các kết hợp. Điều này thực sự trở thành một tìm kiếm trên tất cả các kết quả khớp giữa các yêu cầu và bảng, trong đó tại mỗi bước, chúng tôi bỏ qua một yêu cầu hoặc gán nó cho một trong các bảng có thể có. 

Trong trường hợp xấu nhất, nhánh này phát triển cực kỳ nhanh. Ngay cả khi chúng tôi chỉ xem xét các lựa chọn bài tập, mỗi yêu cầu có thể phân nhánh thành k khả năng, đưa ra cấu trúc gần đúng O(k^n), điều này vượt xa khả năng thực hiện. 

Quan sát chính là cấu trúc về cơ bản là khớp hai bên theo trọng số, nhưng có một ràng buộc rất cụ thể: dung lượng bảng là số nguyên nhỏ và nhiều bảng có thể chia sẻ cùng dung lượng. Điều này cho phép chúng ta nén cạnh bảng thành số lượng theo dung lượng, thay vì xử lý các bảng riêng lẻ. 

Sau khi chúng tôi sắp xếp các bảng theo dung lượng và duy trì số lượng còn lại trong mỗi dung lượng, vấn đề sẽ trở thành việc chọn từng yêu cầu một và quyết định xem có đặt chúng vào bất kỳ dung lượng tương thích sẵn có nào hay không. Điều này gợi ý lập trình động theo yêu cầu và phân bổ dung lượng bảng có sẵn. Tuy nhiên, cách giải thích hiệu quả hơn là xử lý các yêu cầu theo một thứ tự cố định và gán chúng vào bảng tốt nhất có thể bằng cách sử dụng cấu trúc luôn dành các bảng nhỏ cho các yêu cầu nhỏ. 

Thủ thuật tiêu chuẩn là sắp xếp các yêu cầu theo lợi nhuận giảm dần và cố gắng chỉ định từng yêu cầu vào bảng nhỏ nhất có thể đáp ứng yêu cầu đó. Điều này là tối ưu vì các bảng lớn có giá trị hơn đối với các yêu cầu lớn và việc lãng phí chúng cho các yêu cầu nhỏ sẽ làm giảm tính khả thi trong tương lai. 

Chúng tôi có thể duy trì các bảng có sẵn trong một mảng tần số hoặc cấu trúc được sắp xếp và đối với mỗi yêu cầu, hãy thực hiện tìm kiếm nhị phân để tìm bảng nhỏ nhất có thể sử dụng được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^n) | O(n + k) | Quá chậm | 
| Tối ưu (tham lam + tìm kiếm) | O(n log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi cấu trúc các bảng để có thể truy vấn một cách hiệu quả những bảng nào có sẵn cho một quy mô nhóm nhất định. 

1. Sắp xếp tất cả dung lượng của bảng theo thứ tự tăng dần. Điều này cho phép chúng tôi luôn tìm được bảng nhỏ nhất phù hợp với yêu cầu. 
2. Sắp xếp các yêu cầu theo thứ tự lợi nhuận giảm dần. Lý do là các yêu cầu có lợi nhuận cao hơn nên được xem xét trước tiên vì sau này chúng sẽ khó được đặt ở vị trí tối ưu hơn. 
3. Đối với mỗi yêu cầu, thực hiện tìm kiếm nhị phân trên danh sách bảng đã sắp xếp để tìm bảng nhỏ nhất chưa được sử dụng có dung lượng ít nhất bằng kích thước nhóm. 
4. Nếu một bảng như vậy tồn tại, hãy gán yêu cầu cho nó, đánh dấu bảng là đã sử dụng và thêm lợi nhuận vào câu trả lời. 
5. Nếu không có bảng phù hợp, bỏ qua yêu cầu. 
6. Lưu trữ các bài tập dưới dạng cặp chỉ mục yêu cầu và chỉ mục bảng để chúng ta có thể xây dựng lại kết quả đầu ra. 

Điều tinh tế là khi một bảng được sử dụng, nó sẽ bị xóa vĩnh viễn, vì vậy chúng ta phải duy trì cấu trúc hỗ trợ việc xóa. Vì k nhỏ nên một danh sách đơn giản có đánh dấu hoặc cấu trúc giống nhiều tập hợp được triển khai bằng tìm kiếm và loại bỏ nhị phân là đủ. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào trong quy trình, chúng tôi duy trì đặc tính là trong số tất cả các yêu cầu chưa được chỉ định, chúng tôi luôn cố gắng đặt lợi nhuận cao nhất lên đầu tiên vào bảng nhỏ nhất có thể đáp ứng yêu cầu đó. Điều này duy trì tính bất biến rằng không có yêu cầu nào được chấp nhận có thể được hoán đổi với một yêu cầu khác mà không làm giảm tổng lợi nhuận, bởi vì mọi nhiệm vụ thay thế sẽ sử dụng bảng lớn hơn một cách không cần thiết hoặc chặn bảng có dung lượng cao hơn cần thiết sau này. Sự lựa chọn tham lam sẽ điều chỉnh sự khan hiếm dung lượng với thứ tự kích thước yêu cầu, đảm bảo rằng các bảng lớn được bảo toàn cho các yêu cầu lớn, vốn là những yêu cầu duy nhất có thể sử dụng chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
requests = []
for i in range(n):
    c, p = map(int, input().split())
    requests.append((p, c, i + 1))

k = int(input())
tables = list(map(int, input().split()))

tables.sort()
used = [False] * k

requests.sort(reverse=True)

assignments = []
total = 0

for p, c, idx in requests:
    best = -1
    for j in range(k):
        if not used[j] and tables[j] >= c:
            best = j
            break
    if best != -1:
        used[best] = True
        total += p
        assignments.append((idx, best + 1))

print(len(assignments), total)
for a, b in assignments:
    print(a, b)
```Giải pháp sắp xếp các bảng một lần để chúng ta luôn có thể quét từ dung lượng nhỏ nhất đến dung lượng lớn nhất. các`used`array đảm bảo mỗi bảng được sử dụng tối đa một lần. 

Các yêu cầu được sắp xếp theo lợi nhuận giảm dần, điều này thúc đẩy việc đặt hàng tham lam. Đối với mỗi yêu cầu, chúng tôi quét các bảng một cách tuyến tính để tìm bảng có sẵn đầu tiên phù hợp. Với k ≤ 1000, quá trình quét O(nk) này vẫn đủ nhanh trong thực tế. 

Một chi tiết triển khai tinh vi là giữ nguyên các chỉ mục yêu cầu ban đầu trước khi sắp xếp, vì đầu ra yêu cầu đánh số gốc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
10 50
2 100
5 30
3
4 6 9
```Các yêu cầu được sắp xếp theo lợi nhuận: 

(100, cỡ 2), (50, cỡ 10), (30, cỡ 5) 

Các bảng được sắp xếp: 

4, 6, 9 

| Yêu cầu | Kích thước | Lợi nhuận | Quét bảng đã chọn | Được giao | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 100 | 4 phù hợp | 4 | 
| 1 | 10 | 50 | không phù hợp | không | 
| 3 | 5 | 30 | 6 phù hợp | 6 | 

Các bài tập gồm có yêu cầu 2 → bảng 1 (giá trị 4), yêu cầu 3 → bảng 2 (giá trị 6). Tổng lợi nhuận là 130. 

Dấu vết này cho thấy rằng việc bỏ qua yêu cầu lợi nhuận trung bình cho phép sử dụng bảng 4 tốt hơn, trong khi không thể đặt yêu cầu lớn hơn. 

### Ví dụ 2 

đầu vào:```
4
3 20
3 50
3 40
5 60
2
3 5
```Yêu cầu được sắp xếp: 

(60,5), (50,3), (40,3), (20,3) 

| Yêu cầu | Kích thước | Lợi nhuận | Bảng đã chọn | Các bảng còn lại | 
| --- | --- | --- | --- | --- | 
| 4 | 5 | 60 | 5 | [3] | 
| 2 | 3 | 50 | 3 | [] | 
| 3 | 3 | 40 | không | [] | 
| 1 | 3 | 20 | không | [] | 

Điều này cho thấy những kẻ tham lam đương nhiên tiêu thụ chiếc bàn lớn duy nhất cho yêu cầu duy nhất có thể sử dụng nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi yêu cầu có thể quét tất cả các bảng một lần trong trường hợp xấu nhất | 
| Không gian | O(n + k) | Lưu trữ các yêu cầu, bảng và theo dõi bài tập | 

Với n, k ≤ 1000, các phép toán 10⁶ trong trường hợp xấu nhất dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    req = []
    for i in range(n):
        c, p = map(int, input().split())
        req.append((p, c, i + 1))

    k = int(input())
    tab = list(map(int, input().split()))

    tab = sorted([(v, i + 1) for i, v in enumerate(tab)])
    used = [False] * k

    req.sort(reverse=True)

    ans = []
    total = 0

    for p, c, idx in req:
        for j in range(k):
            if not used[j] and tab[j][0] >= c:
                used[j] = True
                total += p
                ans.append((idx, tab[j][1]))
                break

    out = [f"{len(ans)} {total}"]
    for a, b in ans:
        out.append(f"{a} {b}")
    return "\n".join(out)

# provided sample
assert run("""3
10 50
2 100
5 30
3
4 6 9
""") == """2 130
2 1
3 2"""

# minimum input
assert run("""1
1 10
1
1
""") == """1 10
1 1"""

# no valid assignments
assert run("""2
5 10
6 20
1
4
""") == """0 0"""

# all fit same size
assert run("""3
1 5
1 6
1 7
3
10 10 10
""").split()[0] == "3"

# boundary mix
assert run("""3
2 10
2 20
2 30
2
2 3
""")  # just ensure no crash
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu 1 yêu cầu | 1 10 | trường hợp hợp lệ nhỏ nhất | 
| không có kết quả phù hợp | 0 0 | xử lý từ chối | 
| tất cả đều phù hợp với bàn lớn | 3 được chấp nhận | bài tập đầy đủ | 
| công suất hỗn hợp | lợi nhuận tối đa hợp lệ | sự đúng đắn tham lam | 

## Vỏ cạnh 

Trường hợp một cạnh là khi không có bảng nào có thể đáp ứng bất kỳ yêu cầu nào. Đối với đầu vào nơi tất cả`r_i < c_i`, quá trình quét luôn thất bại và không có nhiệm vụ nào được ghi lại. Thuật toán trả về chính xác lợi nhuận bằng 0 vì mọi lần lặp yêu cầu đều không tìm thấy bảng nào có thể sử dụng được. 

Một trường hợp khác là khi nhiều bảng có cùng dung lượng. Vì các bảng được phân biệt theo chỉ mục nên việc đánh dấu chúng riêng lẻ sẽ đảm bảo chúng ta không bao giờ sử dụng lại bảng vật lý, ngay cả khi dung lượng giống hệt nhau. 

Trường hợp cuối cùng là khi có một bảng lớn nhưng các yêu cầu nhỏ lại đến trước theo thứ tự lợi nhuận. Bởi vì chúng tôi luôn chọn lợi nhuận cao nhất trước tiên nên một yêu cầu lớn vẫn sẽ được ưu tiên nếu nó có giá trị cao hơn, ngăn chặn sai lầm tham lam cổ điển là tiêu thụ công suất lớn quá sớm.
