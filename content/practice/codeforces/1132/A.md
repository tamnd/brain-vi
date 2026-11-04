---
title: "CF 1132A - Chuỗi ngoặc thông thường"
description: "Chúng ta có bốn loại chuỗi ngoặc, mỗi loại có độ dài hai: \"((\", \"()\", \")(\", và \"))\". Đầu vào cung cấp số lượng mỗi loại mà chúng tôi có."
date: "2026-06-12T04:07:48+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1132
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 61 (Rated for Div. 2)"
rating: 1100
weight: 1132
solve_time_s: 70
verified: true
draft: false
---

[CF 1132A - Trình tự khung thông thường](https://codeforces.com/problemset/problem/1132/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn loại chuỗi ngoặc, mỗi loại có độ dài hai: "((", "()", ")(", và "))". Đầu vào cung cấp số lượng mỗi loại mà chúng tôi có. Nhiệm vụ của chúng ta là xác định xem liệu có thể nối tất cả các chuỗi này theo thứ tự nào đó để tạo thành một chuỗi dấu ngoặc thông thường hợp lệ hay không. Chuỗi dấu ngoặc thông thường là chuỗi có thể biểu thị biểu thức số học hợp lệ nếu chúng ta chèn "+" và "1" vào giữa các dấu ngoặc. Ví dụ bao gồm "(())" hoặc "()()". Các chuỗi như "))(" hoặc ")((" không hợp lệ vì không có cách nào khớp chính xác dấu ngoặc mở và đóng. 

Mỗi chuỗi đóng góp chính xác hai ký tự, do đó tổng độ dài của chuỗi cuối cùng gấp đôi tổng số đếm. Điều quan trọng cần lưu ý là mỗi loại chuỗi ảnh hưởng đến "sự cân bằng" của dấu ngoặc mở và đóng một cách khác nhau. "((" tăng số dư lên hai, "()" giữ số dư ở mức trung tính, ")(" đầu tiên giảm sau đó tăng số dư và "))" giảm số dư đi hai. 

Các ràng buộc cho phép đếm tới$10^9$, do đó, bất kỳ giải pháp nào cố gắng tạo chuỗi hoặc mô phỏng nối một cách rõ ràng sẽ quá chậm. Chúng ta phải lập luận hoàn toàn bằng cách sử dụng số học và các bất biến thay vì xây dựng trình tự. 

Một trường hợp đặc biệt quan trọng là khi số chuỗi bắt đầu bằng ")" hoặc kết thúc bằng "(" vượt quá số có thể khớp. Ví dụ: nếu chúng ta có một ")(" và không có "((", thì không thể đặt chuỗi đó ở bất kỳ đâu mà không tạo ra số dư âm tại một số điểm. Một trường hợp đặc biệt khác là khi tất cả số đếm đều bằng 0; chuỗi trống là đều đặn. Việc xử lý chính xác các điều kiện biên này là rất quan trọng để vượt qua tất cả các thử nghiệm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng thử tất cả các hoán vị của chuỗi và kiểm tra xem chuỗi được nối có đều đặn hay không. Về nguyên tắc, điều này đúng vì nó khám phá mọi khả năng, nhưng sự tăng trưởng giai thừa của các hoán vị khiến nó không thể thực hiện được ngay cả đối với số lượng chuỗi rất nhỏ. Với số lượng lên tới$10^9$, việc thử hoán vị là không thể. 

Cách tiếp cận tối ưu dựa vào việc duy trì sự cân bằng trong khung. Mỗi chuỗi ngoặc thông thường phải thỏa mãn hai điều kiện. Đầu tiên, tổng số dấu ngoặc mở phải bằng tổng số dấu ngoặc đóng. Thứ hai, quét từ trái sang phải, số dư đang chạy (mở trừ đóng) không bao giờ được âm. Hai điều kiện này có thể được tính trực tiếp từ số đếm mà không cần mô phỏng từng chuỗi. 

Cụ thể, chúng tôi kiểm tra xem số "((" cộng với số ")(" có bằng số "))" cộng với số "((". Điều này đảm bảo tổng số dư bằng 0. Để đáp ứng số dư đang chạy không âm, chênh lệch giữa ")(" và "((" không được vượt quá một tại bất kỳ điểm nào, vì ")(" là chuỗi duy nhất có thể giảm số dư ở giữa chuỗi. Điều này dẫn đến quy tắc ngắn gọn: sự khác biệt tuyệt đối giữa số lượng "((" và "))" phải có nhiều nhất một và nếu có ít nhất một ")(", chúng ta cần một "((" tương ứng để bắt đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các hoán vị) | O((cnt1+cnt2+cnt3+cnt4)!) | O(1) | Quá chậm | 
| Kiểm tra dựa trên số dư | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đếm của 4 loại chuỗi:$cnt1, cnt2, cnt3, cnt4$. Chúng tương ứng với "((", "()", ")(", và "))". 
2. Tính tổng số dấu ngoặc mở như sau$2*cnt1 + cnt2 + cnt3$và tổng số dấu ngoặc đóng như$2*cnt4 + cnt2 + cnt3$. Xác nhận chúng bằng nhau. Nếu không, xuất 0 vì một chuỗi thông thường yêu cầu số dấu ngoặc mở và đóng bằng nhau. 
3. Kiểm tra yêu cầu số dư. Chuỗi khó duy nhất là ")(" vì nó bắt đầu bằng dấu ngoặc đóng. Chúng ta phải đảm bảo có ít nhất một "((" để đặt trước bất kỳ ")(" nào để tránh số dư đang chạy âm khi bắt đầu. Nếu$cnt3 > 0$Và$cnt1 = 0$, đầu ra 0. 
4. Kiểm tra sự khác biệt giữa số lượng "((" và "))". Nếu như$|cnt1 - cnt4| > 1$, xuất ra 0 vì chúng ta không thể giữ số dư hiện hành không âm. 
5. Nếu tất cả các điều kiện đều đạt, xuất 1. 

Điều bất biến được duy trì là tại mọi điểm trong chuỗi được nối, số dư đang chạy của "(" trừ ")" không bao giờ âm và kết thúc bằng 0. Điều này đảm bảo một chuỗi dấu ngoặc thông thường mà không cần mô phỏng phép nối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

cnt1 = int(input())
cnt2 = int(input())
cnt3 = int(input())
cnt4 = int(input())

# check if total number of opening and closing brackets are equal
total_open = 2*cnt1 + cnt2 + cnt3
total_close = 2*cnt4 + cnt2 + cnt3

if total_open != total_close:
    print(0)
elif cnt3 > 0 and cnt1 == 0:
    # cannot start with ")("
    print(0)
elif abs(cnt1 - cnt4) > 1:
    # balance at ends is off by more than 1
    print(0)
else:
    print(1)
```Giải pháp trước tiên sẽ kiểm tra tổng số dư để đảm bảo chuỗi cuối cùng có dấu ngoặc phù hợp. Sau đó, nó xử lý các trường hợp cạnh cho chuỗi ")(" phức tạp và đảm bảo sự khác biệt giữa các chuỗi "((" và đóng thuần túy "))" mở thuần túy không vi phạm số dư hoạt động không âm. Mỗi điều kiện tương ứng trực tiếp với các thuộc tính của chuỗi dấu ngoặc thông thường. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1
4
3
```| Bước | cnt1 | cnt2 | cnt3 | cnt4 | tổng_open | tổng_đóng | Vượt qua kiểm tra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 3 | 1 | 4 | 3 | 2*3+1+4=11 | 2*3+1+4=11 | cân đối nhé | 
| cnt3>0 và cnt1=0? | 4>0 và 3=0? | Không | - | - | - | | | 
| abs(cnt1-cnt4)>1? | cơ bụng(3-3)=0 | - | - | - | - | | | 

Đầu ra: 1. Thuật toán xác nhận điều đó là có thể. 

### Mẫu 2 (chuỗi trống) 

đầu vào:```
0
0
0
0
```| Bước | tổng_open | tổng_đóng | Vượt qua kiểm tra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 0 | 0 | cân đối nhé | 
| cnt3>0 và cnt1=0? | 0>0 và 0=0? | Không | - | 
| abs(cnt1-cnt4)>1? | cơ bụng(0-0)=0 | - | - | 

Đầu ra: 1. Dãy trống là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ số học và so sánh trên bốn số nguyên | 
| Không gian | O(1) | Chỉ tính số lượng cửa hàng và tổng số | 

Ngay cả với số lượng lên tới$10^9$, thuật toán thực hiện một số thao tác cố định, dễ dàng khớp trong giới hạn thời gian 1 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    cnt1 = int(input())
    cnt2 = int(input())
    cnt3 = int(input())
    cnt4 = int(input())

    total_open = 2*cnt1 + cnt2 + cnt3
    total_close = 2*cnt4 + cnt2 + cnt3

    if total_open != total_close:
        return "0"
    elif cnt3 > 0 and cnt1 == 0:
        return "0"
    elif abs(cnt1 - cnt4) > 1:
        return "0"
    else:
        return "1"

# provided samples
assert run("3\n1\n4\n3\n") == "1", "sample 1"
assert run("0\n0\n0\n0\n") == "1", "empty sequence"

# custom cases
assert run("1\n0\n0\n1\n") == "1", "single matching brackets"
assert run("0\n0\n1\n0\n") == "0", "cannot start with )("
assert run("10\n0\n0\n12\n") == "0", "imbalance too large"
assert run("2\n1\n1\n2\n") == "1", "mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 0 1 | 1 | trình tự hợp lệ đơn giản | 
| 0 0 1 0 | 0 | không thể bắt đầu bằng ")(" | 
| 10 0 0 12 | 0 | mất cân bằng giữa "((" và "))" | 
| 2 1 1 2 | 1 | trường hợp hỗn hợp chung với tất cả các loại | 

## Vỏ cạnh 

Khi có ít nhất một ")(" nhưng không có "((", thuật toán trả về 0. Đối với đầu vào`0 0 1 0`,`cnt3>0`Và`cnt1=0`kích hoạt việc kiểm tra. Số dư âm xảy ra ngay lập tức nếu chúng ta cố gắng đặt ")(" ở đầu. Thuật toán phát hiện điều này và đưa ra kết quả chính xác là 0. 

Khi chênh lệch giữa chuỗi mở thuần và chuỗi đóng thuần túy vượt quá 1, chẳng hạn như`10 0 0 12`, thuật toán xuất ra 0 vì chuỗi cuối cùng sẽ yêu cầu nhiều dấu ngoặc mở hơn mức cho phép tại một số điểm, vi phạm bất biến số dư chạy không âm.
