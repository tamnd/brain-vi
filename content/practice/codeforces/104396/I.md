---
title: "CF 104396I - Thang máy"
description: "Trong thang máy có n người, trong đó có bạn. Mỗi người đã chọn một tầng đích. Trong số tất cả các điểm đến đã chọn, có chính xác m tầng riêng biệt xuất hiện."
date: "2026-06-30T23:15:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104396
codeforces_index: "I"
codeforces_contest_name: "2023 Jiangsu Collegiate Programming Contest, 2023 National Invitational of CCPC (Hunan), The 13th Xiangtan Collegiate Programming Contest"
rating: 0
weight: 104396
solve_time_s: 54
verified: true
draft: false
---

[CF 104396I - Thang máy](https://codeforces.com/problemset/problem/104396/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

có`n`người trong thang máy, kể cả chính bạn. Mỗi người đã chọn một tầng đích. Trong số tất cả các điểm đến được chọn, chính xác`m`các tầng riêng biệt xuất hiện. 

Bạn có thể tự do chọn tầng đích của mình và mục tiêu của bạn là tối đa hóa số lượng người, bao gồm cả bạn, rời thang máy cùng nhau trên tầng của bạn. Vì chính xác`m`các tầng khác nhau phải được đại diện, các điểm đến được chỉ định cho các tầng khác`n - 1`mọi người có thể được phân phối theo bất kỳ cách hợp lệ nào đáp ứng được yêu cầu này. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi chỉ được cung cấp tổng số người và số tầng đích riêng biệt. Chúng ta phải tính toán nhóm lớn nhất có thể rời khỏi cùng tầng với chúng ta. 

Các giá trị đầu vào lớn nhất chỉ ở khoảng`10^4`và mọi trường hợp thử nghiệm chỉ chứa hai số nguyên. Kể cả nếu có`10^4`trường hợp thử nghiệm, một`O(1)`giải pháp cho mỗi trường hợp chỉ thực hiện một vài phép tính số học nói chung. Bất kỳ thuật toán phức tạp nào hơn đều không cần thiết vì câu trả lời chỉ phụ thuộc vào một đối số đếm đơn giản. 

Một sai lầm dễ mắc phải là quên rằng mỗi tầng riêng biệt phải có ít nhất một hành khách. 

Ví dụ,```
n = 5
m = 5
```Câu trả lời đúng là`1`. Mỗi hành khách phải chọn một tầng khác nhau để không ai có thể chia sẻ điểm đến của bạn. 

Một sai lầm phổ biến khác là cho rằng tất cả hành khách còn lại luôn có thể chọn tầng của bạn. 

Ví dụ,```
n = 6
m = 3
```Câu trả lời đúng là`4`, không`6`. Ngoài tầng của riêng bạn, vẫn phải có hai tầng riêng biệt khác. Ít nhất một hành khách phải được phân vào mỗi tầng đó, chỉ để lại bốn người trên tầng của bạn. 

Đầu vào nhỏ nhất cũng đáng được chú ý.```
n = 1
m = 1
```Câu trả lời là`1`. Bạn là hành khách duy nhất nên mọi người cùng nhau rời đi. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là tưởng tượng việc phân phối tất cả`n`hành khách trong số chính xác`m`các tầng khác nhau và kiểm tra mọi phân phối hợp lệ. Đối với mỗi lần phân phối, chúng tôi ghi lại nhóm lớn nhất được chỉ định cho một tầng và giữ mức tối đa trong tất cả các khả năng. 

Cách tiếp cận này đúng vì nó xem xét mọi nhiệm vụ pháp lý. Thật không may, số lượng các bản phân phối như vậy tăng theo cấp số nhân với`n`, làm cho nó hoàn toàn không thực tế ngay cả đối với kích thước đầu vào vừa phải. 

Quan sát quan trọng là chúng ta không bao giờ cần biết sự phân bổ chính xác. Để tối đa hóa số lượng người rời đi cùng chúng tôi, mọi hành khách không bị buộc phải lên tầng khác nên chọn tầng của chúng tôi. 

Vì phải có chính xác`m`các tầng riêng biệt, một trong số đó là của chúng tôi, trong khi phần còn lại`m - 1`mỗi tầng yêu cầu ít nhất một hành khách. Chỉ định chính xác một hành khách cho mỗi tầng khác. Điều này sử dụng`m - 1`mọi người rời đi```
n - (m - 1) = n - m + 1
```những người trên sàn của chúng tôi, bao gồm cả chính chúng tôi. 

Không thể có câu trả lời lớn hơn vì mỗi người bổ sung di chuyển lên tầng của chúng tôi sẽ để trống một trong những tầng riêng biệt bắt buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị`n`Và`m`. 
2. Dành riêng một hành khách cho nhau`m - 1`tầng riêng biệt. Đây là nhiệm vụ tối thiểu có thể thực hiện được mà vẫn giữ được tất cả các tầng được yêu cầu. 
3. Đặt mọi hành khách còn lại trên tầng của bạn. Vì không có ràng buộc nào khác tồn tại nên điều này tạo ra nhóm lớn nhất có thể rời đi cùng nhau. 
4. Tính đáp án dưới dạng`n - m + 1`. 
5. Xuất kết quả cho test case hiện tại. 

### Tại sao nó hoạt động 

Mỗi nhiệm vụ hợp lệ phải có ít nhất một hành khách trên mỗi chuyến bay`m - 1`tầng khác với tầng của bạn. Những hành khách đó cũng không thể rời đi cùng bạn. Điều này có nghĩa là ít nhất`m - 1`mọi người bị loại khỏi nhóm của bạn, vì vậy quy mô nhóm của bạn tối đa là`n - (m - 1)`. Thuật toán đạt chính xác giá trị này bằng cách chỉ định một hành khách cho mỗi tầng khác và đặt những người khác vào tầng của bạn. Vì đã đạt được giới hạn trên nên kết quả là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        out.append(str(n - m + 1))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu tiên chương trình sẽ đọc số lượng test case. Mỗi trường hợp thử nghiệm được xử lý độc lập, do đó không cần lưu trữ thông tin giữa các trường hợp. 

Toàn bộ giải pháp là biểu thức số học`n - m + 1`. Điều này diễn ra trực tiếp từ đối số đếm được phát triển trong thuật toán. Không cần vòng qua hành khách hoặc tầng. 

sử dụng`sys.stdin.readline`giữ đầu vào nhanh ngay cả khi số lượng trường hợp thử nghiệm đạt đến mức tối đa. Các câu trả lời được tích lũy thành một danh sách và được in chung ở cuối, tránh nhiều thao tác xuất ra riêng lẻ. 

Không có lo ngại về tràn vì số nguyên Python có độ chính xác tùy ý và các ràng buộc của bài toán vốn đã nhỏ. Điều kiện biên duy nhất là khi`m = n`, trong trường hợp đó công thức đánh giá chính xác thành`1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6
m = 3
```| Bước | n | m | Đặt trước các tầng khác | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 6 | 3 | - | - | 
| Đặt riêng một hành khách cho nhau tầng | 6 | 3 | 2 | - | 
| Hành khách còn lại ở lại với chúng tôi | 6 | 3 | 2 | 4 | 

Hai tầng phụ bắt buộc, mỗi tầng tiếp nhận một hành khách. Bốn hành khách còn lại, bao gồm cả chúng tôi, đều cùng nhau rời đi. 

### Ví dụ 2 

đầu vào:```
n = 5
m = 5
```| Bước | n | m | Đặt trước các tầng khác | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 5 | 5 | - | - | 
| Đặt riêng một hành khách cho nhau tầng | 5 | 5 | 4 | - | 
| Hành khách còn lại ở lại với chúng tôi | 5 | 5 | 4 | 1 | 

Mỗi hành khách phải ở một tầng khác nhau nên không ai có thể chia sẻ điểm đến của chúng tôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có một phép trừ và một phép cộng được thực hiện. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng. | 

Vì mọi trường hợp thử nghiệm chỉ yêu cầu thời gian không đổi và bộ nhớ không đổi nên giải pháp dễ dàng thỏa mãn các ràng buộc ngay cả khi số lượng trường hợp thử nghiệm càng lớn càng tốt. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    ans = []
    for _ in range(t):
        n, m = map(int, input().split())
        ans.append(str(n - m + 1))
    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# custom cases
assert run("1\n1 1\n") == "1", "minimum case"
assert run("1\n6 3\n") == "4", "general case"
assert run("1\n5 5\n") == "1", "all floors distinct"
assert run("1\n10 1\n") == "10", "everyone can leave together"
assert run("1\n10000 9999\n") == "2", "boundary values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Kích thước đầu vào tối thiểu | 
|`6 3`|`4`| Trường hợp đếm chung | 
|`5 5`|`1`| Mỗi tầng phải khác nhau | 
|`10 1`|`10`| Chỉ tồn tại một tầng riêng biệt | 
|`10000 9999`|`2`| Giá trị biên lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể.```
1
1 1
```Thuật toán tính toán`1 - 1 + 1 = 1`. Không có tầng bắt buộc nào khác nên hành khách độc thân sẽ rời đi một mình. Đầu ra là chính xác`1`. 

Bây giờ hãy xem xét trường hợp mỗi hành khách phải chọn một tầng khác nhau.```
1
5 5
```Thuật toán tính toán`5 - 5 + 1 = 1`. Vì cần có bốn tầng riêng biệt khác ngoài tầng của chúng tôi nên mọi hành khách khác phải chọn một trong số đó. Không ai có thể chia sẻ đích đến của chúng ta nên câu trả lời là đúng. 

Cuối cùng, hãy xem xét trường hợp chỉ có một tầng riêng biệt.```
1
8 1
```Thuật toán tính toán`8 - 1 + 1 = 8`. Không cần thêm tầng nên mọi hành khách đều có thể chọn tầng của chúng tôi. Tất cả tám hành khách đều rời đi cùng nhau, điều này rõ ràng là tối ưu.
