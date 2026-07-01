---
title: "CF 104400L - Truy vấn thời gian biểu"
description: "Chúng ta được cung cấp thời gian biểu một ngày có độ dài n, trong đó mỗi phút có đúng một xe buýt đến và mỗi lần đến đều được dán nhãn số xe buýt. Vì vậy, mảng đầu vào là một chuỗi các mã định danh và giá trị thứ i cho chúng ta biết xe buýt nào đến vào phút thứ i."
date: "2026-07-01T00:56:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104400
codeforces_index: "L"
codeforces_contest_name: "Hunan University 2023 the 19th Programming Contest"
rating: 0
weight: 104400
solve_time_s: 47
verified: true
draft: false
---

[CF 104400L - Truy vấn về thời gian biểu](https://codeforces.com/problemset/problem/104400/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một thời gian biểu dài một ngày`n`, trong đó mỗi phút có đúng một xe buýt đến và mỗi lần đến đều được dán nhãn số xe buýt. Vì vậy, mảng đầu vào là một chuỗi các mã định danh và giá trị thứ i cho chúng ta biết xe buýt nào đến vào phút thứ i. 

Sau đó, chúng tôi nhận được`q`truy vấn. Mỗi truy vấn yêu cầu một số xe buýt cụ thể`x`và một vị trí`y`, và chúng ta phải báo cáo phút xảy ra sự cố thứ y của xe buýt`x`xảy ra trong thời gian biểu. Nếu xe buýt đó xuất hiện ít hơn`y`lần, chúng tôi trở lại`-1`. 

Hoạt động cốt lõi không phải là về thời gian mà là về việc lập chỉ mục các lần xuất hiện bên trong các chuỗi con được lọc. Đối với mỗi số xe buýt, về mặt khái niệm, chúng tôi quan tâm đến danh sách tất cả các vị trí mà nó xuất hiện. 

Những hạn chế là`n, q ≤ 100000`. Quét trực tiếp cho mỗi truy vấn sẽ quá chậm. Nếu mỗi truy vấn quét toàn bộ mảng, chúng ta sẽ nhận được`O(nq)`, theo thứ tự 10¹⁰ thao tác trong trường hợp xấu nhất, vượt xa mức 2 giây cho phép trong Python. 

Một hạn chế tinh vi hơn là số bus có thể lớn tới 10⁹, vì vậy chúng ta không thể sử dụng chúng làm chỉ mục mảng trực tiếp. Bất kỳ giải pháp nào cũng phải dựa vào hàm băm hoặc nén phối hợp. 

Một số trường hợp đặc biệt quan trọng ở đây. Nếu xe buýt chỉ xuất hiện một lần, mọi truy vấn yêu cầu lần xuất hiện thứ hai sẽ trả về`-1`. Nếu tất cả các giá trị giống hệt nhau, các truy vấn lớn`y`các giá trị sẽ thường xuyên thất bại. Nếu một chiếc xe buýt không bao giờ xuất hiện thì mọi truy vấn về nó sẽ ngay lập tức`-1`. Những trường hợp này phá vỡ các giải pháp giả định tồn tại mà không kiểm tra độ dài danh sách. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn`(x, y)`, chúng tôi quét toàn bộ thời gian biểu và đếm số lần xuất hiện của`x`cho đến khi chúng ta đến được trận đấu thứ y. Khi chúng tôi nhấn nó, chúng tôi trả lại chỉ mục. Nếu chúng ta hoàn thành mảng trước, chúng ta sẽ quay lại`-1`. 

Điều này đúng vì nó mô phỏng trực tiếp định nghĩa của vấn đề. Vấn đề là chi phí. Mỗi truy vấn có giá`O(n)`thời gian, vượt qua`q`truy vấn chúng tôi thực hiện lên đến`n × q`so sánh. Với cả hai lên tới 100000, con số này trở nên quá lớn. 

Quan sát quan trọng là thời gian biểu không thay đổi. Mọi truy vấn đều hỏi về cùng một chuỗi cố định. Điều đó có nghĩa là chúng ta có thể xử lý trước tất cả các lần xuất hiện một lần, lưu trữ cho mỗi số xe buýt một danh sách các vị trí mà nó xuất hiện. Sau đó, mỗi truy vấn sẽ trở thành một mảng tra cứu đơn giản: phần tử thứ y của danh sách đó, nếu nó tồn tại. 

Điều này chuyển đổi vấn đề từ việc quét lặp đi lặp lại thành truy cập được lập chỉ mục. Công việc nặng nhọc chuyển thành một lần duy nhất trên mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tính toán trước vị trí | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một từ điển ánh xạ từng số xe buýt vào danh sách các vị trí mà nó xuất hiện. Cấu trúc này là cần thiết vì số lượng bus lớn và thưa thớt nên việc lập chỉ mục mảng là không khả thi. 
2. Quét thời gian biểu từ trái sang phải. Với mỗi phút i, hãy thêm i vào danh sách tương ứng với số xe buýt tại vị trí đó. Điều này xây dựng thứ tự xuất hiện chính xác, bắt buộc vì các truy vấn phụ thuộc vào thứ tự thời gian. 
3. Sau khi tiền xử lý, mỗi số bus có một danh sách sắp xếp các chỉ số xuất hiện, đương nhiên theo thứ tự tăng dần vì chúng ta chèn từ trái sang phải. 
4. Đối với mỗi truy vấn`(x, y)`, kiểm tra xem`x`tồn tại trong từ điển. Nếu không thì xuất ngay`-1`bởi vì xe buýt không bao giờ xuất hiện. 
5. Nếu nó tồn tại, hãy kiểm tra xem danh sách có ít nhất`y`các phần tử. Nếu không thì xuất`-1`vì sự xuất hiện được yêu cầu không tồn tại. 
6. Ngược lại xuất phần tử tại chỉ mục`y-1`trong danh sách. Điều này tương ứng trực tiếp với lần xuất hiện thứ y theo thứ tự thời gian. 

Lý do việc lập chỉ mục này hoạt động là vì chúng tôi đã bảo toàn thứ tự thời gian chính xác trong quá trình tiền xử lý, do đó thứ tự danh sách tương đương với thứ tự thời gian. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến đối với mỗi số xe buýt`x`, danh sách được lưu trữ của nó chứa tất cả các chỉ mục trong đó`x`xuất hiện theo thứ tự tăng dần. Vì mọi truy vấn đều yêu cầu lần xuất hiện thứ y theo thời gian, nên điều này tương đương với phần tử thứ y trong danh sách được sắp xếp đó. Không cần sắp xếp lại hoặc tính toán lại, vì vậy mỗi truy vấn sẽ giảm xuống mức tra cứu theo thời gian không đổi trên cấu trúc được tính toán trước chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    pos = {}

    for i, v in enumerate(a, start=1):
        if v not in pos:
            pos[v] = []
        pos[v].append(i)

    out = []

    for _ in range(q):
        x, y = map(int, input().split())
        if x not in pos:
            out.append("-1")
        else:
            lst = pos[x]
            if y <= 0 or y > len(lst):
                out.append("-1")
            else:
                out.append(str(lst[y - 1]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng tiền xử lý xây dựng bản đồ băm được khóa theo số xe buýt, lưu trữ tất cả các chỉ số xuất hiện. Việc sử dụng`enumerate(..., start=1)`đảm bảo chúng tôi lưu trữ các vị trí dựa trên 1, khớp với chỉ mục phút của vấn đề. 

Mỗi truy vấn được xử lý trong thời gian không đổi bằng cách kiểm tra tư cách thành viên từ điển và giới hạn danh sách. Điều kiện biên`y > len(lst)`là cần thiết vì thiếu nó sẽ dẫn đến truy cập ngoài phạm vi hoặc giả định không chính xác về tần số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 5
1 2 3 4 5 5 4 3 2 1
1 1
1 2
1 3
6 1
5 2
```Chúng tôi xây dựng các vị trí: 

| Xe buýt | Vị trí | 
| --- | --- | 
| 1 | [1, 10] | 
| 2 | [2, 9] | 
| 3 | [3, 8] | 
| 4 | [4, 7] | 
| 5 | [5, 6] | 

Bây giờ truy vấn: 

| x | y | danh sách(x) | trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | [1,10] | 1 | 
| 1 | 2 | [1,10] | 10 | 
| 1 | 3 | [1,10] | -1 | 
| 6 | 1 | [] | -1 | 
| 5 | 2 | [5,6] | 6 | 

Đầu ra:```
1
10
-1
-1
6
```Dấu vết này xác nhận rằng việc lập chỉ mục trực tiếp vào các vị trí được lưu trữ khớp với việc đếm số lần xuất hiện theo trình tự thời gian. 

### Ví dụ 2 

đầu vào:```
10 5
1 2 1 2 1 2 3 3 3 1
1 4
1 5
3 3
3 4
2 2
```Vị trí: 

| Xe buýt | Vị trí | 
| --- | --- | 
| 1 | [1, 3, 5, 10] | 
| 2 | [2, 4, 6] | 
| 3 | [7, 8, 9] | 

Truy vấn: 

| x | y | danh sách(x) | trả lời | 
| --- | --- | --- | --- | 
| 1 | 4 | [1,3,5,10] | 10 | 
| 1 | 5 | [1,3,5,10] | -1 | 
| 3 | 3 | [7,8,9] | 9 | 
| 3 | 4 | [7,8,9] | -1 | 
| 2 | 2 | [2,4,6] | 4 | 

Điều này cho thấy cả trường hợp truy xuất thành công và thất bại khi`y`vượt quá tần số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Một lượt để xây dựng danh sách vị trí, sau đó là thời gian không đổi cho mỗi truy vấn | 
| Không gian | O(n) | Mỗi chỉ mục được lưu trữ chính xác một lần trong danh sách | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả tiền xử lý và xử lý truy vấn đều có quy mô tuyến tính với kích thước đầu vào. Với`n, q ≤ 100000`, điều này vẫn ổn trong điều kiện hạn chế về thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    pos = {}
    for i, v in enumerate(a, start=1):
        pos.setdefault(v, []).append(i)

    out = []
    for _ in range(q):
        x, y = map(int, input().split())
        if x not in pos or y > len(pos[x]):
            out.append("-1")
        else:
            out.append(str(pos[x][y - 1]))

    return "\n".join(out)

# provided sample 1
assert run("""10 5
1 2 3 4 5 5 4 3 2 1
1 1
1 2
1 3
6 1
5 2
""") == """1
10
-1
-1
6"""

# provided sample 2
assert run("""10 5
1 2 1 2 1 2 3 3 3 1
1 4
1 5
3 3
3 4
2 2
""") == """10
-1
9
-1
4"""

# single element edge
assert run("""1 2
7
7 1
7 2
""") == """1
-1"""

# all equal
assert run("""5 3
9 9 9 9 9
9 1
9 5
9 6
""") == """1
5
-1"""

# missing value
assert run("""3 2
1 2 3
4 1
4 2
""") == """-1
-1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | kết hợp hợp lệ và không hợp lệ | lập chỉ mục ranh giới | 
| tất cả đều bình đẳng | đầu tiên, cuối cùng, tràn | trường hợp cạnh tần số | 
| giá trị còn thiếu | -1 đầu ra | xử lý khóa vắng mặt | 

## Vỏ cạnh 

Một chiếc xe buýt không bao giờ xuất hiện sẽ được xử lý hoàn toàn thông qua tư cách thành viên từ điển. Khi`x`vắng mặt, việc tra cứu thất bại ngay lập tức và trả về`-1`mà không cần chạm vào bất kỳ danh sách nào. Điều này tránh được lỗi KeyError ngẫu nhiên hoặc các giả định mặc định không chính xác. 

Xe buýt xuất hiện ít lần hơn yêu cầu sẽ dựa vào việc kiểm tra độ dài danh sách được lưu trữ. Ví dụ, nếu`pos[x] = [2, 5]`và truy vấn yêu cầu`y = 3`, điều kiện`y > len(pos[x])`kích hoạt, quay trở lại`-1`. Nếu không có sự kiểm tra này, việc truy cập`pos[x][2]`sẽ sụp đổ. 

Lớn`y`các giá trị đều an toàn vì chúng không bao giờ yêu cầu lặp lại. Thuật toán không bao giờ tính số lần xuất hiện trong các truy vấn, quá cực đoan`y`các giá trị không ảnh hưởng đến hiệu suất hoặc độ chính xác ngoài một lần so sánh.
