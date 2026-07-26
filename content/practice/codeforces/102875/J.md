---
title: "CF 102875J - Chỉ nghịch đảo nhân"
description: "Hàm trong bài toán này là một cách đệ quy để tính nghịch đảo mô đun, nhưng bản thân giá trị được yêu cầu không phải là nghịch đảo."
date: "2026-07-25T13:01:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102875
codeforces_index: "J"
codeforces_contest_name: "2020 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 102875
solve_time_s: 69
verified: true
draft: false
---

[CF 102875J - Nghịch đảo chỉ nhân](https://codeforces.com/problemset/problem/102875/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hàm trong bài toán này là một cách đệ quy để tính nghịch đảo mô đun, nhưng bản thân giá trị được yêu cầu không phải là nghịch đảo. Câu hỏi yêu cầu số lần gọi hàm trung bình khi hàm được khởi động với mọi giá trị bắt đầu có thể có từ`1`ĐẾN`p - 1`, Ở đâu`p`là một số nguyên tố. 

Hàm đệ quy dừng ngay lập tức khi đối số của nó trở thành`1`. Ngược lại, nó di chuyển từ giá trị hiện tại`x`ĐẾN`p mod x`và thực hiện thêm một cuộc gọi nữa. Các phép toán nhân và modulo được sử dụng để xây dựng nghịch đảo không liên quan ở đây vì chỉ có số lượng lệnh gọi là quan trọng. Ví dụ, khi`p = 7`, chuỗi cho`x = 5`là`5 -> 2 -> 1`, vì vậy nó góp phần`3`cuộc gọi đến mức trung bình. 

Đầu vào chứa tối đa 100 giá trị nguyên tố và tối đa mỗi số nguyên tố`10^6`. Giới hạn này là đầu mối thuật toán chính. Một giải pháp cố gắng tuân theo mọi chuỗi đệ quy một cách độc lập có thể lặp lại cùng một bài toán con nhiều lần và việc mô phỏng trực tiếp từng chuỗi sẽ tiếp cận hành vi bậc hai. Đối với một giá trị xung quanh`10^6`, một phương pháp bậc hai sẽ yêu cầu khoảng`10^12`hoạt động vượt xa giới hạn thời gian thông thường cho phép. Chúng ta cần khai thác thực tế là mọi trạng thái chỉ phụ thuộc vào phần dư nhỏ hơn. 

Các trường hợp tế nhị hầu hết đều ở xung quanh ranh giới. Khi`p = 2`, chỉ có một giá trị bắt đầu có thể có,`x = 1`. Câu trả lời là`1`và việc triển khai giả sử vòng lặp luôn chứa các giá trị từ`2`ĐẾN`p - 1`có thể vô tình chia cho 0 vì số giá trị bắt đầu là`p - 1`. 

Đối với đầu vào:```
1
2
```đầu ra đúng là:```
1.0000000000
```Không có chuyển đổi đệ quy vì lệnh gọi duy nhất là`F(1, 2)`. 

Một sai lầm dễ mắc phải khác là coi giá trị mô-đun trả về là câu trả lời. Ví dụ, với`p = 7`Và`x = 5`, hàm trả về giá trị nghịch đảo mô-đun, nhưng phần đóng góp là số lượng lệnh gọi:```
F(5, 7) -> F(2, 7) -> F(1, 7)
```Sự đóng góp là`3`, không phải nghịch đảo của`5`modulo`7`. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng hàm đệ quy cho mọi giá trị bắt đầu có thể. Đối với mỗi`x`từ`1`ĐẾN`p - 1`, chúng tôi liên tục thay thế`x`với`p mod x`cho đến khi đạt được`1`, đếm số lượng cuộc gọi. Điều này cho kết quả chính xác vì đệ quy mô tả chính xác quá trình chúng ta cần đo. 

Vấn đề là nhiều giá trị ban đầu khác nhau có cùng giá trị trung gian. Nếu chúng ta tính toán từng chuỗi riêng biệt thì phần còn lại có thể được đánh giá lại nhiều lần. Trong trường hợp xấu nhất, thực hiện việc này một cách độc lập với tất cả các giá trị ban đầu có thể trở thành phương trình bậc hai. Với`p`gần với`10^6`, điều đó có nghĩa là khoảng một nghìn tỷ lần chuyển đổi. 

Quan sát quan trọng là biểu đồ phụ thuộc đã được sắp xếp sẵn. Đối với bất kỳ`x > 1`, giá trị tiếp theo là`p mod x`và giá trị này luôn nhỏ hơn`x`. Điều đó có nghĩa là nếu chúng ta tính toán câu trả lời theo thứ tự tăng dần của`x`, câu trả lời cho mọi chuyển đổi đều đã được biết trước. 

Cho phép`dp[x]`là số lượng cuộc gọi được thực hiện bởi hàm bắt đầu từ`x`. Trường hợp cơ bản là`dp[1] = 1`. Đối với mọi giá trị khác:$$dp[x] = 1 + dp[p \bmod x]$$phần bổ sung`1`đại diện cho lệnh gọi hiện tại và các lệnh gọi còn lại chính xác là các lệnh gọi từ đệ quy con. 

Câu trả lời cuối cùng là trung bình cộng của tất cả`dp[x]`giá trị từ`1`ĐẾN`p - 1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(p2) | O(1) | Quá chậm | 
| Tối ưu | O(p) | O(p) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`dp`Ở đâu`dp[x]`lưu trữ số lượng cuộc gọi cần thiết khi chức năng bắt đầu từ`x`. Bộ`dp[1] = 1`vì hàm trả về ngay lập tức cho trạng thái này. Điều này trực tiếp cho chúng ta điều kiện dừng duy nhất. 
2. Lặp lại`x`từ`2`ĐẾN`p - 1`. Với mỗi giá trị, hãy tính trạng thái đệ quy tiếp theo`p mod x`. Vì giá trị này nhỏ hơn`x`, câu trả lời của nó đã được tính toán. 
3. Đặt`dp[x] = 1 + dp[p mod x]`. Cuộc gọi hiện tại đóng góp một và phần còn lại của chuỗi đã được lưu trữ trong bảng lập trình động. 
4. Thêm mọi`dp[x]`giá trị và chia tổng số cho`p - 1`. Mọi giá trị bắt đầu từ`1`ĐẾN`p - 1`có xác suất bằng nhau nên kỳ vọng cần có là giá trị trung bình số học. 

Tại sao nó hoạt động: bất biến quan trọng là sau khi xử lý mọi giá trị lên tới`x`,`dp[i]`là đúng với mọi`i <= x`. Sự chuyển đổi đệ quy từ`x`luôn di chuyển đến một số nhỏ hơn, do đó bài toán con cần thiết đã được bao hàm bởi bất biến. Bởi vì mọi giá trị ban đầu có thể được bao gồm chính xác một lần trong tổng cuối cùng, nên mức trung bình được tính toán khớp với số lượng cuộc gọi dự kiến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(p):
    if p == 2:
        return 1.0

    dp = [0] * p
    dp[1] = 1

    total = 1
    for x in range(2, p):
        dp[x] = 1 + dp[p % x]
        total += dp[x]

    return total / (p - 1)

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        p = int(input())
        ans.append(f"{solve_case(p):.10f}")
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Trường hợp đặc biệt`p = 2`tránh tạo ra một phép tính trung bình không hợp lệ. Chỉ có một giá trị bắt đầu có thể, vì vậy câu trả lời là trực tiếp`1`. 

Đối với các số nguyên tố lớn hơn,`dp`có kích thước`p`bởi vì mọi giá trị bắt đầu có thể đều nhỏ hơn`p`. Thứ tự vòng lặp tăng dần là chi tiết triển khai trung tâm. Khi tính toán`dp[x]`, chỉ số`p % x`được đảm bảo nhỏ hơn`x`, do đó không cần đệ quy hoặc yêu cầu đặt hàng bổ sung. 

Biến`total`được giữ dưới dạng số nguyên trong khi tính tổng. Việc chuyển đổi sang dấu phẩy động chỉ xảy ra trong phép chia cuối cùng, giúp tránh mất độ chính xác không cần thiết trong quá trình tích lũy. 

## Ví dụ đã hoạt động 

cho`p = 7`, các giá trị được tính toán là: 

| x | p mod x | dp[x] | Tổng chạy | 
| --- | --- | --- | --- | 
| 1 | - | 1 | 1 | 
| 2 | 1 | 2 | 3 | 
| 3 | 1 | 2 | 5 | 
| 4 | 3 | 3 | 8 | 
| 5 | 2 | 3 | 11 | 
| 6 | 1 | 2 | 13 | 

Trung bình là:$$13 / 6 = 2.1666666667$$Ví dụ này cho thấy các bài toán con lặp lại được sử dụng lại như thế nào. Các dây chuyền cho`4`Và`5`không cần phải tính toán lại một cách độc lập các đường đi qua`3`Và`2`. 

Vì`p = 5`: 

| x | p mod x | dp[x] | Tổng chạy | 
| --- | --- | --- | --- | 
| 1 | - | 1 | 1 | 
| 2 | 1 | 2 | 3 | 
| 3 | 2 | 3 | 6 | 
| 4 | 1 | 2 | 8 | 

Kết quả cuối cùng là:$$8 / 4 = 2.0$$Trường hợp này chứng tỏ rằng một trạng thái có thể phụ thuộc vào một giá trị khác không trực tiếp trước đó.`dp[3]`phụ thuộc vào`dp[2]`, vốn đã có sẵn vì phép lặp tuân theo thứ tự tăng dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(p) | Mọi giá trị từ`1`ĐẾN`p - 1`được xử lý một lần. | 
| Không gian | O(p) | Mảng lập trình động lưu trữ một giá trị cho mọi trạng thái có thể. | 

Số nguyên tố lớn nhất có thể là`10^6`, do đó thuật toán thực hiện khoảng một triệu thao tác đơn giản cho mỗi trường hợp thử nghiệm. Điều này phù hợp một cách thoải mái vì công việc lặp đi lặp lại từ các mô phỏng đệ quy đã bị loại bỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_case(p):
        if p == 2:
            return 1.0
        dp = [0] * p
        dp[1] = 1
        total = 1
        for x in range(2, p):
            dp[x] = 1 + dp[p % x]
            total += dp[x]
        return total / (p - 1)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(f"{solve_case(int(input())):.10f}")
    return "\n".join(out)

assert solution("""5
2
3
5
7
999983
""") == """1.0000000000
1.5000000000
2.0000000000
2.1666666667
15.9864347558""", "samples"

assert solution("""1
2
""") == "1.0000000000", "minimum prime"

assert solution("""1
3
""") == "1.5000000000", "small recursive case"

assert solution("""1
5
""") == "2.0000000000", "small chain reuse"

assert solution("""1
7
""") == "2.1666666667", "branching chains"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`1.0000000000`| Xử lý số nguyên tố nhỏ nhất có thể và tránh các lỗi chia. | 
|`3`|`1.5000000000`| Kiểm tra các chuyển đổi đệ quy không tầm thường đầu tiên. | 
|`5`|`2.0000000000`| Xác thực thứ tự phụ thuộc trong mảng DP. | 
|`7`|`2.1666666667`| Bài tập nhiều chuỗi chia sẻ trạng thái trung gian. | 

## Vỏ cạnh 

cho`p = 2`, thuật toán trả về ngay`1.0`. Vòng lặp từ`2`ĐẾN`p - 1`trống, nhưng điều này đúng vì điểm bắt đầu duy nhất là`x = 1`. 

Đối với đầu vào:```
1
2
```việc thực hiện là: 

| Bước | Tiểu bang | 
| --- | --- | 
| Bắt đầu | Giá trị duy nhất là`x = 1`| 
| Gọi hàm | Một cuộc gọi | 
| Trung bình |`1 / 1 = 1`| 

Đối với trường hợp giá trị bắt đầu đạt đến trạng thái tính toán khác, thứ tự DP sẽ xử lý nó một cách tự nhiên. Với`p = 7`, khi tính`dp[5]`, quá trình chuyển đổi là:```
5 -> 2
```giá trị`dp[2]`đã được biết:```
dp[2] = 1 + dp[1] = 2
```Vì thế:```
dp[5] = 1 + dp[2] = 3
```Thuật toán không bao giờ tính toán lại chuỗi`2 -> 1`, đó là lý do chính khiến giải pháp tuyến tính hoạt động.
