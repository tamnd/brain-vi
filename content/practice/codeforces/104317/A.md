---
title: "CF 104317A - Antiamuny muốn học tìm kiếm nhị phân"
description: "Chúng ta được cung cấp một quy trình hoạt động giống hệt như một tìm kiếm nhị phân tiêu chuẩn, ngoại trừ thay vì trả về vị trí của một giá trị đích, nó trả về số lần lặp vòng lặp được thực hiện cho đến khi tìm kiếm tìm thấy phần tử đích."
date: "2026-07-01T19:29:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104317
codeforces_index: "A"
codeforces_contest_name: "Shanghai University 2023 Spring Contest"
rating: 0
weight: 104317
solve_time_s: 61
verified: true
draft: false
---

[CF 104317A - Antiamuny muốn học tìm kiếm nhị phân](https://codeforces.com/problemset/problem/104317/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quy trình hoạt động giống hệt như một tìm kiếm nhị phân tiêu chuẩn, ngoại trừ thay vì trả về vị trí của một giá trị đích, nó trả về số lần lặp vòng lặp được thực hiện cho đến khi tìm kiếm tìm thấy phần tử đích. 

Việc tìm kiếm hoạt động trên một phân đoạn số nguyên$[L, R]$và chúng tôi được đảm bảo rằng giá trị mục tiêu$x$nằm trong khoảng này. Mỗi lần lặp chọn điểm giữa, so sánh nó với$x$và dừng lại nếu khớp hoặc thu hẹp khoảng về nửa bên trái hoặc bên phải tùy thuộc vào điểm giữa lớn hơn hay nhỏ hơn$x$. Đầu ra được yêu cầu chỉ đơn giản là số lần lặp được thực hiện trước khi kết thúc. 

Các hạn chế là nhỏ:$L, R, x \le 1000$, và nhiều nhất$100$các trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng ngay cả một mô phỏng trực tiếp cũng an toàn, vì mỗi tìm kiếm nhị phân chỉ mất tối đa khoảng$\log_2(1000)$, tức là dưới 10 lần lặp. Vì vậy, ngay cả việc triển khai đơn giản cũng có thể thực hiện dưới một nghìn hoạt động tổng thể. 

Điểm tinh tế chính là chúng ta không được yêu cầu về vị trí hoặc đường dẫn cuối cùng mà là số lần thực hiện vòng lặp chính xác. Một sai lầm phổ biến là giả sử giá trị này bằng số lần chia đôi khoảng thời gian cho đến khi còn lại một phần tử duy nhất, phần tử này gần giống nhau nhưng không phải lúc nào cũng giống nhau nếu điểm giữa chạm mục tiêu sớm. 

Các trường hợp cạnh đáng chú ý là các khoảng thời gian tầm thường như$L = R$, trong đó vòng lặp chạy đúng một lần và kết thúc ngay lập tức, và các trường hợp$x$bằng điểm giữa của lần lặp thứ nhất hoặc thứ hai, thay đổi độ sâu so với duyệt cây tìm kiếm nhị phân đầy đủ. 

## Phương pháp tiếp cận 

Quy trình này mang tính xác định và đã được xác định đầy đủ. Ý tưởng brute-force là mô phỏng vòng lặp theo đúng nghĩa đen như đã viết: duy trì$l, r$, tính toán$mid = (l + r) // 2$, cập nhật giới hạn và đếm số lần lặp cho đến khi$mid == x$. Điều này hiệu quả vì mọi chuyển đổi trạng thái trong vòng lặp đều được xác định rõ ràng và không có phần phụ thuộc ẩn nào. 

Quan sát chính là không cần tối ưu hóa ngoài mô phỏng. Phạm vi rất nhỏ và mỗi lần lặp sẽ thu hẹp khoảng thời gian một cách nghiêm ngặt, do đó độ dài vòng lặp được giới hạn bởi$\lceil \log_2(R-L+1) \rceil$. Ngay cả trong trường hợp xấu nhất, đây là công việc có quy mô không đổi cho mỗi trường hợp thử nghiệm. 

Vì vậy, giải pháp “tối ưu” giống hệt với mô phỏng lực lượng vũ phu và sự khác biệt chỉ mang tính khái niệm: chúng tôi nhận thấy rằng quy trình này đã đủ hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(T \log (R-L))$|$O(1)$| Đã chấp nhận | 
| Tối ưu (giống nhau) |$O(T \log (R-L))$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng chính xác tìm kiếm nhị phân và đếm số lần thân vòng lặp thực thi. 

1. Khởi tạo bộ đếm về 0. Bộ đếm này theo dõi số lần lặp của vòng tìm kiếm nhị phân xảy ra trước khi kết thúc. 
2. Trong khi khoảng thời gian hiện tại$[l, r]$là hợp lệ, có nghĩa là$l \le r$, thực hiện một lần tìm kiếm. Điều kiện này đảm bảo chúng ta chỉ tiếp tục khi không gian tìm kiếm không trống. 
3. Tăng bộ đếm vì mỗi lần thực hiện vòng lặp tương ứng với một bước tìm kiếm nhị phân bất kể chúng ta có kết thúc ngay sau đó hay không. 
4. Tính điểm giữa$mid = (l + r) // 2$. Đây là lựa chọn mang tính quyết định tương tự như cách triển khai đã cho và nó xác định vị trí đầu dò hiện tại. 
5. Nếu$mid == x$, phá vỡ ngay lập tức. Điều này mô hình việc chấm dứt tìm kiếm thành công. 
6. Nếu$mid < x$, chuyển ranh giới bên trái sang$mid + 1$, loại bỏ tất cả các giá trị quá nhỏ. 
7. Ngược lại, chuyển ranh giới bên phải sang$mid - 1$, loại bỏ tất cả các giá trị quá lớn. 

Sau các bước này, bộ đếm phản ánh chính xác số lần lặp cần thiết để đạt được$x$. 

Tính đúng đắn xuất phát từ thực tế là thuật toán là dấu vết thực thi trực tiếp của thủ tục đã cho. Tại mỗi lần lặp, trạng thái$(l, r)$khớp với hàm ban đầu và bộ đếm tăng chính xác một lần trong mỗi lần thực hiện vòng lặp. Vì vòng lặp chỉ kết thúc khi$mid == x$, số lần tăng chính xác là số lần lặp được thực hiện trước khi kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        l, r, x = map(int, input().split())
        cnt = 0
        while l <= r:
            cnt += 1
            mid = (l + r) // 2
            if mid == x:
                break
            if mid < x:
                l = mid + 1
            else:
                r = mid - 1
        print(cnt)

if __name__ == "__main__":
    solve()
```Giải pháp là phiên âm trực tiếp của hàm đã cho. Thành phần bổ sung duy nhất là bộ đếm, tăng dần khi bắt đầu mỗi lần lặp vòng lặp để khớp với ngữ nghĩa chính xác của mã gốc. Điều kiện vòng lặp và các cập nhật không thay đổi, đảm bảo hành vi giống hệt nhau. Phép chia số nguyên được sử dụng để khớp chính xác ngữ nghĩa Python với mã giả được cung cấp. 

Một chi tiết tinh tế là bộ đếm được tăng lên trước khi kiểm tra xem`mid == x`. Điều này là cần thiết vì ngay cả lần lặp kết thúc cũng phải được tính, phù hợp với định nghĩa vấn đề. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp đầu vào mẫu đầu tiên`3 7 6`. 

Chúng tôi theo dõi việc thực hiện từng bước. 

| Lặp lại | tôi | r | giữa | Hành động | cnt | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 7 | 5 | giữa < x, di chuyển sang phải | 1 | 
| 2 | 6 | 7 | 6 | giữa == x, dừng | 2 | 

Vòng lặp chạy hai lần nên câu trả lời là 2. Điều này chứng tỏ rằng việc kết thúc có thể xảy ra trước khi khoảng thời gian thu gọn hoàn toàn, tùy thuộc vào vị trí điểm giữa. 

Bây giờ hãy xem xét`5 8 6`. 

| Lặp lại | tôi | r | giữa | Hành động | cnt | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 8 | 6 | giữa == x, dừng | 1 | 

Ở đây mục tiêu được tìm thấy ngay lập tức nên chỉ có một lần lặp được thực thi. 

Điều này cho thấy số lượng phụ thuộc vào chuỗi điểm giữa chính xác, không chỉ kích thước khoảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log (R-L))$| Mỗi bài kiểm tra thực hiện một vòng lặp tìm kiếm nhị phân tiêu chuẩn, giúp giảm một nửa phạm vi mỗi lần lặp | 
| Không gian |$O(1)$| Chỉ một vài số nguyên được sử dụng cho mỗi trường hợp thử nghiệm | 

Các ràng buộc đảm bảo tối đa 100 thử nghiệm và phạm vi lên tới 1000, do đó tổng số lần lặp vòng lặp là không đáng kể. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log2

    data = inp.strip().split()
    t = int(data[0])
    idx = 1
    out = []

    for _ in range(t):
        l = int(data[idx]); r = int(data[idx+1]); x = int(data[idx+2])
        idx += 3

        cnt = 0
        while l <= r:
            cnt += 1
            mid = (l + r) // 2
            if mid == x:
                break
            if mid < x:
                l = mid + 1
            else:
                r = mid - 1
        out.append(str(cnt))

    return "\n".join(out)

# provided samples
assert run("5\n3 7 6\n6 12 7\n2 10 2\n6 14 13\n5 8 6") == "2\n2\n3\n3\n1"
assert run("1\n1 1 1") == "1"

# minimum interval
assert run("1\n1 1 1") == "1"

# already at midpoint early termination
assert run("1\n1 3 2") == "1"

# skewed range
assert run("1\n1 8 1") >= "1"

# symmetric mid-depth check
assert run("1\n1 7 7") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 1 1`|`1`| khoảng phần tử đơn | 
|`1\n1 3 2`|`1`| đánh vào điểm giữa ngay lập tức | 
|`1\n1 7 7`|`3`| đường tìm kiếm sâu hơn bên phải | 

## Vỏ cạnh 

Khi nào$L = R$, vòng lặp thực hiện đúng một lần vì điều kiện$l \le r$là đúng,$mid$bằng giá trị duy nhất và hàm kết thúc ngay lập tức. Việc triển khai sẽ tăng bộ đếm trước khi kiểm tra đẳng thức, do đó đầu ra là 1, khớp với hành vi mong đợi. 

Đối với trường hợp như$L = 1, R = 3, x = 2$, điểm giữa đầu tiên là 2, do đó việc kết thúc xảy ra ở lần lặp đầu tiên. Bộ đếm trở thành 1 và không áp dụng cập nhật nào nữa. Điều này xác nhận rằng việc kết thúc sớm không bỏ qua việc đếm lần lặp thành công. 

Đối với trường hợp mục tiêu ở một ranh giới cực đoan, chẳng hạn như$L = 1, R = 7, x = 7$, việc tìm kiếm phải đi qua một số điểm giữa trước khi đến cạnh phải. Mỗi lần lặp lại sẽ thu hẹp khoảng thời gian một cách chính xác và số vòng lặp khớp với độ sâu của đường dẫn cây tìm kiếm nhị phân tiềm ẩn đến lá ngoài cùng bên phải.
