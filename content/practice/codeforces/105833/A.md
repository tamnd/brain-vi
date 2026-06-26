---
title: "CF 105833A - Trò chơi chống chéo"
description: "Chúng ta có một chuỗi S có độ dài N + 1, trong đó mỗi vị trí trên đường chéo của lưới được gắn nhãn A hoặc B. Mã thông báo bắt đầu tại (0, 0) trong lưới (N + 1) × (N + 1). Người chơi luân phiên di chuyển và mỗi nước đi sẽ tăng chỉ số hàng hoặc chỉ mục cột thêm một."
date: "2026-06-26T03:55:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105833
codeforces_index: "A"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2025"
rating: 0
weight: 105833
solve_time_s: 52
verified: true
draft: false
---

[CF 105833A - Trò chơi chống đường chéo](https://codeforces.com/problemset/problem/105833/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi`S`chiều dài`N + 1`, trong đó mỗi vị trí trên đường chéo của lưới được dán nhãn`A`hoặc`B`. 

Mã thông báo bắt đầu lúc`(0, 0)`trong một`(N + 1) × (N + 1)`lưới. Người chơi luân phiên di chuyển và mỗi nước đi sẽ tăng chỉ số hàng hoặc chỉ mục cột thêm một. Trò chơi dừng ngay khi mã thông báo đến một ô`(i, j)`với`i + j = N`. 

Tế bào chống đường chéo`(i, N - i)`tương ứng với ký tự`S[i]`. 

Nếu nhân vật đó là`A`, Alice thắng. Nếu nó là`B`, Bob thắng. 

Alice đi trước và cả hai người chơi đều chơi tối ưu. Nhiệm vụ không phải là giải quyết một trường hợp trò chơi đơn lẻ. Thay vào đó, trong số tất cả`2^(N+1)`chuỗi có thể`S`, chúng ta phải đếm xem có bao nhiêu người khiến Alice trở thành người chiến thắng. Câu trả lời là bắt buộc theo modulo`10^9 + 3233`. 

Hạn chế chính là`N ≤ 100000`. Bất kỳ giải pháp nào kiểm tra tất cả các chuỗi đều không thể thực hiện được. Ngay cả một chương trình động trên toàn bộ biểu đồ trò chơi cũng không cần thiết. Giải pháp dự định là một đối số đếm dạng đóng, dẫn đến thời gian logarit vì chỉ cần lũy thừa mô-đun. 

Một sai lầm phổ biến là nghĩ rằng mọi ký tự của chuỗi đều quan trọng. Trong thực tế, lối chơi tối ưu sẽ thu gọn trò chơi xuống chỉ còn hai vị trí trung tâm khi`N`là số lẻ và chỉ có ba vị trí trung tâm khi`N`là chẵn. Phần còn lại của chuỗi hoàn toàn không liên quan. 

Ví dụ, với`N = 1`, chuỗi có độ dài`2`.```
AA
AB
BA
BB
```Alice có thể chọn trực tiếp điểm cuối đối chéo. Cô ấy thắng trong ba trường hợp đầu tiên và chỉ thua trong`BB`. 

Vì`N = 2`, chỉ có bộ ba`(S0,S1,S2)`vấn đề. Các chuỗi chiến thắng là:```
AAA
AAB
BAA
```phù hợp với câu trả lời mẫu`3`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả`2^(N+1)`dây. Đối với mỗi chuỗi, chúng ta có thể giải trò chơi bằng minimax trên lưới. Điều này đúng vì nó tuân theo định nghĩa trò chơi. Thật không may, ngay cả đối với`N = 50`, số lượng dây vốn đã lớn về mặt thiên văn nên hướng đi này là vô vọng. 

Sự đột phá đến từ sự hiểu biết về hình học của trò chơi. 

Sau chính xác`N`di chuyển, mã thông báo đạt đến đường chéo. Đường dẫn thực tế chỉ xác định ô đối diện nào đạt được. Cách chơi tối ưu cho phép người chơi kiểm soát sự cân bằng giữa di chuyển hàng và di chuyển cột. 

Đầu tiên hãy xem xét một giá trị lẻ`N = 2K + 1`. Alice thực hiện bước đi cuối cùng. Chiến lược ghép nối cho thấy rằng sau khi di chuyển, cô ấy luôn có thể buộc mã thông báo vào một trong hai ô đối chéo trung tâm, tương ứng với các chỉ số`K`Và`K + 1`. Ngược lại, nếu cả hai ô đó đều thua cô ấy, Bob có thể buộc trò chơi sao cho nước đi cuối cùng của Alice phải chọn giữa chính xác hai ô thua cuộc đó. Do đó Alice thắng khi và chỉ nếu có ít nhất một trong các`S[K]`hoặc`S[K+1]`là`A`. 

Bây giờ hãy xem xét một giá trị chẵn`N = 2K`. Alice chọn nước đi đầu tiên, sau đó Bob trở thành người chơi đầu tiên trong một trò chơi con có độ dài lẻ. Nếu Alice di chuyển một chiều, cặp trung tâm có liên quan sẽ trở thành`(S[K-1], S[K])`. Nếu cô ấy di chuyển theo hướng khác, cặp trung tâm có liên quan sẽ trở thành`(S[K], S[K+1])`. Bob thắng trò chơi phụ lẻ trừ khi cả hai ô trong cặp trung tâm của nó đều`A`. Do đó, Alice thắng chính xác khi có ít nhất một trong các cặp này bao gồm toàn bộ`A`. 

Đối với số lẻ`N`, trong số bốn nhiệm vụ của cặp trung tâm, có ba nhiệm vụ chiến thắng:```
AA, AB, BA
```Chỉ một`BB`thua cuộc. 

Thậm chí`N`, trong số tám nhiệm vụ của bộ ba trung tâm`(S[K-1], S[K], S[K+1])`, có đúng ba người chiến thắng:```
AAA, AAB, BAA
```Số đếm trở thành: 

Đối với số lẻ`N = 2K + 1`:```
3 * 2^(N-1)
= 3 * 4^K
```Thậm chí`N = 2K`:```
3 * 2^(N-2)
= 3 * 4^(K-1)
```Cả hai biểu thức đều đơn giản hóa thành```
3 * 4^floor((N-1)/2)
```đó chính xác là công thức cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(N+1)) | O(1) | Quá chậm | 
| Tối ưu | O(log N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`N`. 
2. Tính toán`e = floor((N - 1) / 2)`. 
3. Tính toán`4^e mod (10^9 + 3233)`sử dụng lũy ​​thừa mô đun nhanh. 
4. Nhân kết quả với`3`. 
5. Lấy giá trị cuối cùng theo modulo`10^9 + 3233`. 
6. Xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Đối với số lẻ`N`, cách chơi tối ưu chỉ phụ thuộc vào hai vị trí đối chéo trung tâm. Alice thắng trừ khi cả hai đều được gắn nhãn`B`, đưa ra chính xác`3`giành được nhiệm vụ đảm nhiệm hai vị trí đó. 

Thậm chí`N`, lối chơi tối ưu chỉ phụ thuộc vào ba vị trí trung tâm. Alice chiến thắng chính xác trong ba nhiệm vụ`AAA`,`AAB`, Và`BAA`. 

Tất cả các ký tự còn lại của chuỗi không bao giờ ảnh hưởng đến kết quả và có thể được chọn tự do. Sau khi đếm các vị trí trống, cả hai trường hợp chẵn lẻ đều thu gọn về dạng đóng giống nhau:$$3 \cdot 4^{\lfloor (N-1)/2 \rfloor}.$$Thuật toán đánh giá chính xác công thức này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 3233

def main():
    n = int(input())
    exp = (n - 1) // 2
    ans = 3 * pow(4, exp, MOD)
    print(ans % MOD)

if __name__ == "__main__":
    main()
```Việc thực hiện gần như là một bản dịch trực tiếp của công thức toán học.`exp = (n - 1) // 2`tính số mũ xuất hiện ở dạng đóng. Ba đối số tích hợp của Python`pow`thực hiện phép lũy thừa mô-đun theo thời gian logarit, nhanh hơn nhiều so với phép nhân liên tục. 

Phép nhân với`3`được thực hiện sau đó và phép toán modulo cuối cùng giữ kết quả trong phạm vi yêu cầu. 

Không có điều kiện biên phức tạp. Đầu vào nhỏ nhất là`N = 1`, cho số mũ`0`, Và`pow(4, 0, MOD)`trả về một cách chính xác`1`, đưa ra câu trả lời`3`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```Đây`N = 2`. 

| Biến | Giá trị | 
| --- | --- | 
|`exp`|`(2 - 1) // 2 = 0`| 
|`4^exp mod M`|`1`| 
| Trả lời |`3 * 1 = 3`| 

Đầu ra:```
3
```Đây chính xác là mẫu. Bộ ba chiến thắng duy nhất là`AAA`,`AAB`, Và`BAA`. 

### Ví dụ 2 

đầu vào:```
5
```| Biến | Giá trị | 
| --- | --- | 
|`exp`|`(5 - 1) // 2 = 2`| 
|`4^exp mod M`|`16`| 
| Trả lời |`3 * 16 = 48`| 

Đầu ra:```
48
```Vì`N = 5`, chỉ có hai vị trí trung tâm là quan trọng. Ba trong số bốn nhiệm vụ của họ đều chiến thắng và bốn nhân vật còn lại được tự do, mang lại`3 * 2^4 = 48`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log N) | lũy thừa mô-đun | 
| Không gian | O(1) | Bộ nhớ bổ sung liên tục | 

Với`N`lên đến`100000`, điều này dễ dàng nằm trong giới hạn. Thời gian chạy bị chi phối bởi một tính toán công suất mô-đun duy nhất. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 10**9 + 3233

def solve():
    n = int(input())
    print((3 * pow(4, (n - 1) // 2, MOD)) % MOD)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("2\n") == "3\n", "sample"

# minimum N
assert run("1\n") == "3\n", "N=1"

# odd N
assert run("3\n") == "12\n", "3 * 4^1"

# even N
assert run("4\n") == "12\n", "3 * 4^1"

# larger value
assert run("5\n") == "48\n", "3 * 4^2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`3`| Đầu vào nhỏ nhất có thể | 
|`2`|`3`| Trường hợp mẫu | 
|`3`|`12`| Số lẻ`N`công thức | 
|`4`|`12`| Thậm chí`N`công thức | 
|`5`|`48`| Số mũ lớn hơn | 

## Vỏ cạnh 

cho`N = 1`, trò chơi kết thúc sau nước đi đầu tiên của Alice. 

đầu vào:```
1
```Cặp liên quan chỉ đơn giản là`(S0,S1)`. Alice chỉ thua khi cả hai đều`B`. có`3`chuỗi chiến thắng và công thức cho:$$3 \cdot 4^0 = 3.$$Vì`N = 2`, chỉ có ba vấn đề trung tâm. 

đầu vào:```
2
```Bộ ba chiến thắng là:```
AAA
AAB
BAA
```Chính xác có ba bài tập làm được nên câu trả lời là`3`. 

Đối với một giá trị lẻ lớn như`N = 99999`, sẽ không thể liệt kê được các chuỗi. Thuật toán vẫn chỉ thực hiện một phép tính lũy thừa mô-đun:$$3 \cdot 4^{49999} \pmod{10^9+3233}.$$Thời gian chạy vẫn ở dạng logarit theo số mũ, đủ nhanh.
