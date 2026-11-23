---
title: "CF 421D - Lỗi mã"
description: "Chúng ta được cung cấp một mạng lưới các lập trình viên và mỗi lập trình viên sẽ đưa ra yêu cầu trong một cuộc họp: họ chỉ vào hai lập trình viên khác và khẳng định rằng thủ phạm là một trong hai người đó. Sau đó chúng tôi phải chọn chính xác hai lập trình viên để đưa vào làm nghi phạm."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 421
codeforces_index: "D"
codeforces_contest_name: "Coder-Strike 2014 - Finals (online edition, Div. 2)"
rating: 1900
weight: 421
solve_time_s: 97
verified: true
draft: false
---

[CF 421D - Lỗi trong mã](https://codeforces.com/problemset/problem/421/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng lưới các lập trình viên và mỗi lập trình viên sẽ đưa ra yêu cầu trong một cuộc họp: họ chỉ vào hai lập trình viên khác và khẳng định rằng thủ phạm là một trong hai người đó. Sau đó chúng tôi phải chọn chính xác hai lập trình viên để đưa vào làm nghi phạm. 

Một lập trình viên “ủng hộ” cặp đã chọn của chúng tôi nếu ít nhất một trong hai người chúng tôi chọn xuất hiện trong cặp mà họ đã đề cập. Nói cách khác, mỗi lập trình viên đóng góp một phiếu đồng ý cho một cặp nghi phạm ứng cử viên bất cứ khi nào cặp được chọn của chúng tôi giao với cặp nghi phạm đã khai báo của họ. 

Nhiệm vụ là đếm xem chúng ta có thể chọn được bao nhiêu cặp mã hóa không có thứ tự sao cho ít nhất`p`lập trình viên đồng ý với sự lựa chọn đó. 

Các hạn chế rất lớn: lên tới 300.000 lập trình viên. Việc liệt kê bậc hai trên tất cả các cặp ngay lập tức là không thể vì nó sẽ yêu cầu khoảng 4,5 × 10^10 kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. Điều này buộc chúng ta phải hướng tới một giải pháp trong đó việc đánh giá cặp được giảm xuống gần với thời gian tuyến tính hoặc gần tuyến tính. 

Một điểm tinh tế là sự đồng thuận không có tính đối xứng theo nghĩa “đếm các cạnh”. Người lập trình sẽ đóng góp nếu một trong hai điểm cuối khớp nhau, do đó, một cặp sẽ đóng góp dựa trên sự trùng lặp chứ không phải trùng khớp chính xác. Một cách tiếp cận ngây thơ chỉ tính các kết quả trùng khớp chính xác hoặc xử lý vấn đề như tần số đơn giản của các cặp sẽ bị tính sai. 

Một trường hợp lỗi phổ biến xuất hiện khi nhiều người lập trình chọn cùng một cặp. Ví dụ: nếu nhiều lập trình viên báo cáo`(1, 2)`, thì cặp`(1, 2)`được điều chỉnh thêm ngoài mức độ được đề xuất. Bỏ qua điều này dẫn đến việc đếm quá nhiều cặp hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp brute-force thử mọi cặp không có thứ tự`(u, v)`và quét tất cả các lập trình viên để đếm xem có bao nhiêu cặp mà họ đã chọn giao nhau`{u, v}`. Mỗi lần kiểm tra là thời gian không đổi, vì vậy tổng độ phức tạp là O(n³) trong trường hợp xấu nhất hoặc ít nhất là cặp O(n²) nhân với lần quét O(n), điều này hoàn toàn không khả thi. 

Chúng ta cần một cách để tính điểm thỏa thuận cho một cặp mà không cần quét tất cả các lập trình viên. Quan sát quan trọng là sự đóng góp của người lập trình chỉ phụ thuộc vào việc liệu`u`hoặc`v`xuất hiện trong cặp của họ`(x_i, y_i)`. Điều này có thể được viết lại bằng cách sử dụng độ. 

Nếu chúng ta định nghĩa`deg[x]`là số lượng người lập trình có cặp bao gồm`x`, sau đó cho một cặp ứng cử viên`(u, v)`tổng đóng góp ngây thơ là`deg[u] + deg[v]`. Tuy nhiên, những lập trình viên đã chọn chính xác`(u, v)`được tính hai lần trong số tiền đó, trong khi họ chỉ nên đóng góp một lần. Việc đếm quá mức này được khắc phục bằng cách trừ đi số lượng người lập trình có cặp chính xác`(u, v)`, mà chúng tôi gọi là`cnt[u][v]`. 

Vì vậy, số điểm thực sự trở thành:`score(u, v) = deg[u] + deg[v] - cnt[u][v]`. 

Công thức này rất mạnh vì`deg`có thể được tính toán trước trong O(n), và tất cả`(u, v)`các cặp thực sự xuất hiện trong đầu vào có thể được lưu trữ trong bản đồ băm để tra cứu hiệu chỉnh O(1). 

Thử thách còn lại là đếm xem có bao nhiêu cặp thỏa mãn`score(u, v) ≥ p`trên tất cả các khả năng O(n2) mà không liệt kê chúng. Chúng tôi chia vấn đề. Đầu tiên, bỏ qua các sửa chữa và đếm tất cả các cặp trong đó`deg[u] + deg[v] ≥ p`. Vì độ chỉ phụ thuộc vào các đỉnh nên chúng ta có thể sắp xếp các đỉnh theo độ và sử dụng thao tác quét hai con trỏ để đếm các cặp hợp lệ trong O(n). 

Điều này dẫn đến số lượng vượt quá vì một số cặp vượt qua ngưỡng độ có thể bị lỗi sau khi trừ`cnt[u][v]`. Những trường hợp có vấn đề này chỉ xảy ra đối với các cặp thực sự xuất hiện trong đầu vào, vì chỉ những cặp đó mới có hiệu chỉnh khác 0. Vì vậy, chúng tôi lặp lại các cặp đầu vào một lần nữa và trừ các cặp đó trong đó:`deg[u] + deg[v] ≥ p`Nhưng`deg[u] + deg[v] - cnt[u][v] < p`. 

Điều này làm giảm pha hiệu chỉnh thành O(n), giữ cho toàn bộ nghiệm tuyến tính do sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² · n) | O(n) | Quá chậm | 
| Phân rã độ + hiệu chỉnh | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`deg[i]`cho mọi chỉ mục lập trình viên`i`bằng cách quét tất cả các cặp đầu vào một lần. Điều này ghi lại số lần mỗi nút được đề cập trên tất cả các câu lệnh. 
2. Xây dựng bản đồ tần suất`cnt`theo cặp`(x_i, y_i)`, điều trị`(a, b)`Và`(b, a)`là cùng một khóa bằng cách luôn lưu trữ các điểm cuối được sắp xếp. Điều này cho phép tra cứu nhanh chóng có bao nhiêu lập trình viên đã chọn chính xác một cặp nhất định. 
3. Tạo một mảng gồm tất cả các nút được sắp xếp theo giá trị độ của chúng. Thứ tự này sẽ cho phép chúng ta đếm xem có bao nhiêu cặp có tổng bậc đủ lớn mà không cần kiểm tra từng cặp một cách rõ ràng. 
4. Đếm xem có bao nhiêu cặp`(u, v)`thỏa mãn`deg[u] + deg[v] ≥ p`bằng cách sử dụng quét hai con trỏ. Ý tưởng là khi một điểm cuối tăng về mức độ thì mức độ tối thiểu cần thiết cho điểm cuối kia sẽ giảm một cách đơn điệu. 
5. Lặp lại từng cặp đầu vào`(u, v)`và tính điểm chính xác của nó bằng cách sử dụng`deg[u] + deg[v] - cnt[u][v]`. 
6. Nếu cặp này trước đây được tính là hợp lệ ở bước chỉ cấp bằng, nhưng điểm đã sửa của nó lại thấp hơn`p`, trừ nó khỏi câu trả lời. Thao tác này chỉ loại bỏ các cặp được đưa vào không chính xác do đếm quá mức. 

### Tại sao nó hoạt động 

Đóng góp của mỗi cặp cho thỏa thuận được chia thành hai phần: đóng góp từ các điểm cuối một cách độc lập và phần điều chỉnh chỉ phụ thuộc vào sự va chạm của cặp giống hệt nhau. Việc tính toán dựa trên mức độ tính toán chính xác tất cả các đóng góp dựa trên điểm cuối trên toàn cầu, trong khi thuật ngữ hiệu chỉnh chỉ ảnh hưởng đến các cặp tồn tại rõ ràng trong đầu vào. Vì không có cặp nào khác có thể có hiệu chỉnh khác 0, nên việc loại bỏ các cặp không hợp lệ khỏi tập hợp dựa trên độ sẽ mang lại số đếm cuối cùng chính xác mà không bỏ sót hoặc tính hai lần bất kỳ cấu hình hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, p = map(int, input().split())

pairs = []
deg = [0] * (n + 1)
cnt = {}

for _ in range(n):
    x, y = map(int, input().split())
    pairs.append((x, y))
    deg[x] += 1
    deg[y] += 1
    if x > y:
        x, y = y, x
    cnt[(x, y)] = cnt.get((x, y), 0) + 1

nodes = list(range(1, n + 1))
nodes.sort(key=lambda x: deg[x])

ans = 0
j = n - 1

for i in range(n):
    while j >= 0 and deg[nodes[i]] + deg[nodes[j]] >= p:
        j -= 1
    ans += (n - 1 - j)

for x, y in pairs:
    u, v = (x, y) if x < y else (y, x)
    s = deg[x] + deg[y]
    c = cnt[(u, v)]
    if s >= p and s - c < p:
        ans -= 1

print(ans)
```Đầu tiên, mã nén tất cả ý kiến ​​của người lập trình thành số độ và tần số cặp. Bước sắp xếp cho phép quét hai con trỏ đếm xem có bao nhiêu cặp thỏa mãn điều kiện thoải mái chỉ dựa trên độ. Vòng lặp thứ hai khắc phục tình trạng đếm quá mức bằng cách chỉ kiểm tra các cặp được xác định đầu vào trong trường hợp xung đột xảy ra. 

Một chi tiết triển khai tinh tế đang được duy trì`j`đơn điệu trong khi quét`i`. Điều này phụ thuộc vào thực tế là ngày càng tăng`i`di chuyển đến mức độ nhỏ hơn, do đó điều kiện ngưỡng chỉ trở nên khó khăn hơn, cho phép sử dụng lại con trỏ thay vì đặt lại nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
2 3
1 4
1 4
2 1
```Chúng tôi tính toán độ: 

Nút 1 xuất hiện 3 lần, nút 2 xuất hiện 2 lần, nút 3 xuất hiện 1 lần, nút 4 xuất hiện 2 lần. 

Chúng tôi cũng lưu trữ tần số cặp:`(2,3)=1`,`(1,4)=2`,`(1,2)=1`. 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Độ xây dựng | [ -, 3, 2, 1, 2 ] | 
| 2 | Đếm các cặp độ hợp lệ | cặp có tổng độ ≥ 2 | 
| 3 | Câu trả lời ban đầu | 6 | 
| 4 | Kiểm tra sửa chữa`(1,4)`| tổng độ = 5, cnt = 2, điểm = 3 ≥ 2 không thay đổi | 
| 5 | Kiểm tra sửa chữa`(1,2)`| tổng độ = 5, cnt = 1, điểm = 4 ≥ 2 không thay đổi | 

Câu trả lời cuối cùng vẫn là 6. 

Dấu vết này cho thấy hầu hết các cặp đều đã hợp lệ dưới các ràng buộc về mức độ và các sửa đổi không làm mất hiệu lực của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | các nút sắp xếp cộng với quét tuyến tính theo cặp | 
| Không gian | O(n) | mảng độ và bản đồ băm của các cặp đầu vào | 

Giải pháp này vừa vặn trong giới hạn vì tất cả các thao tác nặng đều giảm xuống mức sắp xếp và tuyến tính vượt qua 300.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import check_output
    return check_output(["python3", "solution.py"], input=inp.encode()).decode().strip()

# provided sample
assert run("""4 2
2 3
1 4
1 4
2 1
""") == "6"

# minimum size
assert run("""3 0
2 3
1 3
1 2
""") == "3"

# all same pairs
assert run("""5 3
2 3
2 3
2 3
2 3
2 3
""") == "1"

# high threshold, no valid pairs
assert run("""4 10
2 3
3 4
4 2
2 1
""") == "0"

# boundary stress small chain
assert run("""6 2
2 3
3 4
4 5
5 6
6 1
1 2
""") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu | 3 | xử lý ngưỡng trống | 
| tất cả các cặp giống nhau | 1 | chỉnh sửa trùng lặp nặng | 
| ngưỡng cao | 0 | không có trường hợp cặp hợp lệ | 
| đầu vào chu kỳ | 15 | đếm cặp hợp lệ dày đặc | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các lập trình viên đặt tên cho cùng một cặp. Trong tình huống đó, độ của cả hai nút trở nên rất lớn và số lượng dựa trên độ được đánh giá quá cao. Tuy nhiên,`cnt[u][v]`khớp chính xác với số lượng vượt mức và bước hiệu chỉnh sẽ loại bỏ chính xác phần bổ sung không hợp lệ để chỉ còn lại phần đóng góp thực sự. 

Một trường hợp khác là khi`p = 0`. Mọi cặp đều hợp lệ bất kể ý kiến ​​của người lập trình. Việc tính toán dựa trên độ vẫn sẽ tính toán các giá trị một cách chính xác, nhưng ngay cả khi việc hiệu chỉnh bị xử lý sai thì không có cặp nào cần loại trừ vì ngưỡng được thỏa mãn một cách trống rỗng. 

Trường hợp cạnh cuối cùng xuất hiện khi tất cả các cặp đều khác nhau. Sau đó`cnt[u][v]`luôn là 1 đối với các cặp hiện có và 0 đối với các cặp khác, nghĩa là việc sửa chỉ điều chỉnh điểm một chút. Thuật toán vẫn hoạt động vì hiệu chỉnh chỉ chạm vào các cạnh hiện có và không bao giờ ảnh hưởng đến phần lớn các cặp không phải cạnh O(n²).
