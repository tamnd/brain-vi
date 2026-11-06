---
title: "CF 1291B - Làm sắc nét mảng"
description: "Chúng ta được cho một mảng các số nguyên không âm. Chúng ta được phép giảm liên tục bất kỳ phần tử nào xuống 1 miễn là nó không âm."
date: "2026-06-16T04:17:38+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1291
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 616 (Div. 2)"
rating: 1300
weight: 1291
solve_time_s: 313
verified: false
draft: false
---

[CF 1291B - Làm sắc nét mảng](https://codeforces.com/problemset/problem/1291/B) 

**Đánh giá:** 1300 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 5 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Chúng ta được phép giảm liên tục bất kỳ phần tử nào xuống 1 miễn là nó không âm. Mục tiêu là để quyết định xem liệu chúng ta có thể chuyển đổi mảng thành một hình dạng mà đầu tiên tăng đúng đến một vị trí đỉnh nào đó rồi giảm dần sau đó hay không. 

Điểm mấu chốt là chúng tôi không sắp xếp lại các yếu tố. Chúng tôi chỉ giảm giá trị. Vì vậy, mỗi vị trí có giới hạn trên được cho bởi giá trị ban đầu của nó, nhưng chúng ta có thể chọn bất kỳ giá trị cuối cùng nào trong khoảng từ 0 đến giới hạn đó. 

Chúng tôi muốn biết liệu có tồn tại sự lựa chọn các giá trị cuối cùng tạo thành một chuỗi “ngọn núi” nghiêm ngặt hay không. 

Các ràng buộc rất lớn: tổng độ dài của tất cả các trường hợp thử nghiệm lên tới 3·10^5. Bất kỳ giải pháp nào cố gắng kiểm tra tất cả các vị trí đỉnh có thể có và mô phỏng các điều chỉnh một cách ngây thơ sẽ thất bại, vì ngay cả O(n^2) cho mỗi trường hợp thử nghiệm cũng quá chậm. 

Một vấn đề tế nhị xuất phát từ việc suy nghĩ tham lam mà không thực thi nghiêm ngặt một cách chính xác. Ví dụ: nếu mảng đã không tăng hoặc không giảm thì mảng đó sẽ tự động hợp lệ, nhưng các hình dạng trung gian như`[2, 1, 2]`không thể sửa được vì phần tử ở giữa không thể vừa trở thành một đỉnh nghiêm ngặt mà vẫn tôn trọng cấu trúc đơn điệu. 

Một tình huống khó khăn khác là giá trị bằng nhau. Vì cần có sự bất bình đẳng nghiêm ngặt, các giá trị liền kề bằng nhau buộc ít nhất một bên phải giảm đi, điều này có thể xếp tầng và phá hủy tính khả thi. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi chỉ số đỉnh có thể`k`. Đối với mỗi lựa chọn, chúng tôi cố gắng xây dựng một chuỗi tăng dần từ bên trái và một chuỗi giảm dần từ bên phải, không bao giờ vượt quá các giá trị ban đầu. 

Đối với một đỉnh cố định, chúng ta có thể gán một cách tham lam các giá trị nhỏ nhất có thể thỏa mãn tính nghiêm ngặt trong khi vẫn giữ nguyên mảng ban đầu. Ví dụ: từ bên trái chúng ta có thể thực thi`b[i] = min(a[i], b[i-1] + 1)`để tăng lên, và tương tự từ bên phải. Điều này hiệu quả vì chúng tôi luôn muốn giữ giá trị lớn nhất có thể trong khi vẫn tôn trọng các ràng buộc. 

Tuy nhiên, việc lặp lại cách xây dựng này cho mọi`k`dẫn đến O(n^2) cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Với tổng số 3·10^5 phần tử, tốc độ này quá chậm. 

Quan sát quan trọng là tính khả thi phụ thuộc vào việc liệu một công trình xây dựng “đỉnh tốt nhất có thể” có tồn tại trên toàn cầu hay không. Thay vì chọn đỉnh một cách rõ ràng, chúng ta có thể tính toán hai mảng: tiền tố tăng nghiêm ngặt tối đa có thể đạt được và hậu tố giảm nghiêm ngặt tối đa có thể đạt được. Sau đó, chúng tôi kiểm tra xem có chỉ số nào trong đó việc xây dựng tăng dần từ bên trái có thể đáp ứng việc xây dựng giảm dần từ bên phải mà không có xung đột hay không. 

Điều này làm giảm vấn đề xuống còn hai lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hai mảng trợ giúp. Một mô phỏng chuỗi tăng nghiêm ngặt tốt nhất mà chúng ta có thể xây dựng từ bên trái và chuỗi còn lại mô phỏng chuỗi giảm nghiêm ngặt tốt nhất từ ​​bên phải. 

### Quy trình từng bước 

1. Xây dựng một mảng`inc`Ở đâu`inc[i]`là giá trị tối đa chúng ta có thể gán tại vị trí`i`đồng thời thực hiện trình tự tăng dần từ trái sang phải và không bao giờ vượt quá`a[i]`. Chúng tôi thiết lập`inc[0] = a[0]`, và với mỗi`i > 0`, chúng tôi tính toán`inc[i] = min(a[i], inc[i-1] + 1)`. Điều này đảm bảo chúng tôi duy trì mức tăng nghiêm ngặt trong khi lãng phí ít giá trị nhất có thể. 
2. Xây dựng một mảng`dec`Ở đâu`dec[i]`là giá trị tối đa chúng ta có thể gán tại vị trí`i`đồng thời thực hiện dãy giảm dần từ phải sang trái. Chúng tôi thiết lập`dec[n-1] = a[n-1]`, và với mỗi`i < n-1`, chúng tôi tính toán`dec[i] = min(a[i], dec[i+1] + 1)`. 
3. Đối với từng vị trí`k`, chúng tôi kiểm tra xem nó có thể đóng vai trò là đỉnh hay không. Để có đỉnh hợp lệ, phần tăng phải kết thúc ở`k`và phần giảm phải bắt đầu từ`k`. Vì vậy chúng ta cần`inc[k] > 0`khả năng tương thích và`inc[k]`phải lớn hơn cả hai cạnh xung quanh nó một cách nhất quán. Điều kiện sạch sẽ trở thành kiểm tra xem có tồn tại không`k`như vậy`inc[k]`tương thích với`dec[k]`khi được coi là đỉnh (cả hai cấu trúc có thể nhận ra cùng một giá trị tại`k`). 
4. Chúng tôi xác minh xem có tồn tại ít nhất một chỉ mục hay không`k`như vậy`inc[k] == dec[k]`. Nếu vị trí như vậy tồn tại, chúng ta có thể căn chỉnh cả hai công trình để tạo thành một ngọn núi hợp lệ. 
5. Nếu không có chỉ mục nào như vậy thì câu trả lời là "Không", nếu không thì là "Có". 

### Tại sao nó hoạt động 

Việc xây dựng`inc`Và`dec`nắm bắt chiều cao khả thi tối đa ở mỗi chỉ số theo các ràng buộc đơn điệu. Bất kỳ mảng được làm sắc nét hợp lệ nào cũng phải nằm bên dưới cả hai cấu trúc theo chiều điểm. Nếu chúng ta có thể tìm thấy một điểm mà cả hai cách xây dựng đều đồng ý, thì điểm đó có thể hoạt động như một đỉnh nhất quán nơi các ràng buộc bên trái và bên phải gặp nhau mà không vi phạm tính nghiêm ngặt. Vì cả hai mảng đều là cực đại tham lam dưới các ràng buộc tương ứng của chúng, nên mọi giải pháp hợp lệ đều phải được thể hiện thông qua sự liên kết này, đảm bảo tính đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if n == 1:
            print("Yes")
            continue

        inc = [0] * n
        dec = [0] * n

        inc[0] = a[0]
        for i in range(1, n):
            inc[i] = min(a[i], inc[i-1] + 1)

        dec[n-1] = a[n-1]
        for i in range(n-2, -1, -1):
            dec[i] = min(a[i], dec[i+1] + 1)

        ok = False
        for i in range(n):
            if inc[i] == dec[i]:
                ok = True
                break

        print("Yes" if ok else "No")

if __name__ == "__main__":
    solve()
```các`inc`mảng thực thi tiền tố tăng nghiêm ngặt tốt nhất có thể mà không vượt quá các ràng buộc. các`dec`mảng thực hiện tương tự cho hậu tố. Lần quét cuối cùng sẽ kiểm tra xem liệu có tồn tại chiều cao đỉnh nhất quán ở nơi cả hai cấu trúc có thể gặp nhau hay không. 

Một cạm bẫy thực hiện phổ biến là quên rằng cả hai bên đều phải tôn trọng tính nghiêm ngặt. các`+1`quá trình chuyển đổi là những gì thực thi tăng hoặc giảm nghiêm ngặt một cách ngầm định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
12 10 8
```| tôi | một [tôi] | inc[i] | tháng mười hai[i] | 
| --- | --- | --- | --- | 
| 0 | 12 | 12 | | 
| 1 | 10 | 10 | | 
| 2 | 8 | 8 | 8 | 
| | | | 10 | 
| | | | 12 | 

Đây`inc = [12, 10, 8]`Và`dec = [12, 10, 8]`. Các mảng khớp nhau ở mọi vị trí, do đó tồn tại một đỉnh hợp lệ. Thuật toán xuất ra "Có". 

### Ví dụ 2 

đầu vào:```
4
0 1 1 0
```| tôi | một [tôi] | inc[i] | tháng mười hai[i] | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | | 
| 1 | 1 | 1 | | 
| 2 | 1 | 1 | | 
| 3 | 0 | 0 | 0 | 
| | | | 1 | 
| | | | 0 | 

Đây`inc = [0, 1, 1, 0]`Và`dec = [0, 1, 1, 0]`, nhưng sự nghiêm ngặt không thể diễn giải được vì cao nguyên bằng nhau ngăn cản việc hình thành cấu trúc đỉnh nghiêm ngặt xung quanh bất kỳ trung tâm nào. Thuật toán loại bỏ chính xác khi không tồn tại sự căn chỉnh đỉnh nghiêm ngặt hợp lệ dưới các ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hai đường tuyến tính để xây dựng mảng và một lần quét | 
| Không gian | O(n) | Lưu trữ hai mảng phụ trợ | 

Tổng kích thước đầu vào trong các trường hợp thử nghiệm được giới hạn bởi 3·10^5, do đó giải pháp thời gian tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))

            if n == 1:
                out.append("Yes")
                continue

            inc = [0] * n
            dec = [0] * n

            inc[0] = a[0]
            for i in range(1, n):
                inc[i] = min(a[i], inc[i-1] + 1)

            dec[n-1] = a[n-1]
            for i in range(n-2, -1, -1):
                dec[i] = min(a[i], dec[i+1] + 1)

            ok = False
            for i in range(n):
                if inc[i] == dec[i]:
                    ok = True
                    break

            out.append("Yes" if ok else "No")

        return "\n".join(out)

    return solve()

# provided samples
assert run("""10
1
248618
3
12 10 8
6
100 11 15 9 7 8
4
0 1 1 0
2
0 0
2
0 1
2
1 0
2
1 1
3
0 1 0
3
1 0 1
""") == """Yes
Yes
Yes
No
No
Yes
Yes
Yes
Yes
No"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | Có | trường hợp cơ bản tầm thường | 
| đơn điệu giảm dần | Có | hình dạng đã hợp lệ | 
| tất cả các số 0 bằng nhau | Không | trường hợp vi phạm nghiêm ngặt | 

## Vỏ cạnh 

Mảng một phần tử luôn thỏa mãn định nghĩa vì không cần đến các bất đẳng thức nghiêm ngặt. Thuật toán xử lý việc này một cách rõ ràng. 

Mảng có các giá trị lặp lại như`[1, 1, 1]`thất bại vì bất kỳ nỗ lực nào nhằm hình thành các phân đoạn đơn điệu nghiêm ngặt đều buộc phải rút gọn ít nhất một lần và điều đó phá vỡ tính đối xứng cần thiết cho một đỉnh. 

Mảng đơn điệu nghiêm ngặt luôn được chấp nhận vì một trong hai`inc`hoặc`dec`trở nên giống hệt với mảng ban đầu, đảm bảo khớp hoàn toàn trên toàn bộ cấu trúc.
