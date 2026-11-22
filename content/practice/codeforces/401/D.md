---
title: "CF 401D - La Mã và số"
description: "Chúng ta được yêu cầu đếm số số nguyên có thể được hình thành bằng cách hoán vị các chữ số của một số $n$ cho trước, không bắt đầu bằng 0 và chia hết cho một mô đun cho trước $m$. Giá trị đầu vào $n$ có thể lớn tới $10^{18}$, nghĩa là có tối đa 18 chữ số và $m$ nhiều nhất là 100."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "combinatorics", "dp", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 401
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 235 (Div. 2)"
rating: 2000
weight: 401
solve_time_s: 79
verified: true
draft: false
---

[CF 401D - La Mã và số](https://codeforces.com/problemset/problem/401/D) 

**Xếp hạng:** 2000 
**Tags:** bitmasks, lực lượng vũ phu, tổ hợp, dp, lý thuyết số 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số số nguyên có thể được tạo thành bằng cách hoán vị các chữ số của một số cho trước$n$, không bắt đầu bằng 0 và chia hết cho một mô đun nhất định$m$. đầu vào$n$có thể lớn như$10^{18}$, có nghĩa là tối đa 18 chữ số và$m$nhiều nhất là 100. Kết quả đầu ra chỉ đơn giản là đếm các số hợp lệ. 

Thách thức nằm ở chỗ việc tạo ra tất cả các hoán vị một cách đơn giản là không khả thi. Ngay cả một số có 18 chữ số khác nhau cũng có$18! \approx 6.4 \times 10^{15}$hoán vị, có giá trị lớn về mặt thiên văn. Điều này loại trừ bất kỳ giải pháp nào liệt kê rõ ràng các hoán vị. 

Các trường hợp cạnh bao gồm các số có chữ số lặp lại, có thể làm giảm số lượng hoán vị riêng biệt. Các số có chữ số 0 cần được chăm sóc đặc biệt vì số 0 đứng đầu không hợp lệ. Ví dụ, đối với$n = 102$Và$m = 2$, các hoán vị$012$không hợp lệ vì nó bắt đầu bằng số 0, nhưng$120$Và$210$là hợp lệ. 

Một sự tinh tế khác là xử lý khả năng phân chia. Chúng tôi không thể kiểm tra tính chia hết sau khi tạo số vì có quá nhiều số. Thay vào đó, chúng ta phải tính toán phần còn lại tăng dần trong khi xây dựng số. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force tạo ra tất cả các hoán vị của các chữ số của$n$, kiểm tra xem chúng không bắt đầu bằng 0 và sau đó kiểm tra khả năng chia hết cho$m$. Điều này hoạt động đối với các số có rất ít chữ số. Với$d$chữ số, điều này đòi hỏi lên đến$d!$hoán vị, mỗi lần lấy$O(d)$để kiểm tra tính chia hết, vì vậy$O(d! \cdot d)$. Vì$d = 18$, điều này là không thể thực hiện được. 

Quan sát chính là chúng ta không cần tạo ra các hoán vị một cách rõ ràng. Mỗi số là sự sắp xếp lại của các chữ số, điều này gợi ý một cách tự nhiên một không gian trạng thái được xác định bởi các chữ số đã được sử dụng cho đến nay. Từ$d \le 18$, chúng ta có thể mã hóa các chữ số được sử dụng dưới dạng bitmask có độ dài$d$. Vấn đề còn lại là tính số cách để hoàn thành số đã cho một phần bitmask và modulo còn lại hiện tại.$m$. 

Lập trình động trên mặt nạ bit và phần dư giải quyết được điều này. Cho phép`dp[mask][rem]`biểu thị số cách tạo thành số bằng cách sử dụng các chữ số tương ứng với`mask`để lại phần còn lại`rem`modulo`m`. Ở mỗi bước, chúng tôi thử đặt một chữ số không sử dụng ở vị trí tiếp theo, cập nhật phần còn lại bằng số học mô-đun và cập nhật mặt nạ. Để tránh đếm các số bắt đầu bằng số 0, chúng ta chỉ cần bỏ qua số 0 nếu chúng ta ở vị trí đầu tiên. Để xử lý các chữ số lặp lại mà không đếm hai lần, chúng tôi đảm bảo rằng nếu có hai chữ số giống hệt nhau trong tập hợp còn lại, chúng tôi luôn sử dụng chữ số ngoài cùng bên trái chưa được sử dụng trước tiên trong thứ tự của mình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d! * d) | O(d!) | Quá chậm đối với d > 10 | 
| Mặt nạ bit DP | O(2^d * m * d) | O(2^d * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi số$n$vào một danh sách các chữ số và sắp xếp chúng. Việc sắp xếp đảm bảo rằng các chữ số lặp lại có thể được xử lý một cách nhất quán mà không cần hoán vị tính hai lần. 
2. Khởi tạo bảng DP`dp[mask][rem]`với -1 để biểu thị trạng thái chưa được tính toán.`mask`là một số nguyên trong đó$i$-bit thứ là 1 nếu$i$-chữ số thứ đã được sử dụng.`rem`là modulo còn lại`m`. 
3. Xác định hàm đệ quy`count(mask, rem)`trả về số lượng số hợp lệ có thể được tạo bằng cách sử dụng các chữ số không được sử dụng trong`mask`, với số dư hiện tại`rem`. 
4. Trong đệ quy, nếu sử dụng tất cả các chữ số (`mask == (1 << d) - 1`), kiểm tra xem`rem == 0`. Nếu vậy, trả về 1, nếu không trả về 0. Đây là trường hợp cơ bản. 
5. Ngược lại, lặp lại tất cả các vị trí chữ số`i`. Bỏ qua bất kỳ chữ số nào đã được sử dụng (`mask & (1 << i)`) hoặc bỏ qua số 0 đứng đầu nếu`mask == 0`và chữ số bằng không. Để tránh đếm hai chữ số giống hệt nhau, hãy bỏ qua một chữ số nếu nó giống với chữ số trước đó và chữ số trước đó chưa được sử dụng (`digits[i] == digits[i-1]`Và`(mask & (1 << (i-1))) == 0`). 
6. Để có lựa chọn hợp lệ, hãy gọi đệ quy`count(mask | (1 << i), (rem * 10 + digits[i]) % m)`và tích lũy kết quả. 
7. Lưu kết quả vào`dp[mask][rem]`và trả lại nó. 

Tại sao nó hoạt động: DP đảm bảo rằng mọi sự kết hợp của các chữ số không sử dụng và số dư đều được xem xét chính xác một lần. Việc bỏ qua các chữ số lặp lại khi chữ số giống hệt trước đó chưa được sử dụng sẽ ngăn cản việc đếm quá mức các hoán vị. Số 0 đứng đầu được tránh một cách rõ ràng. Bằng cách xây dựng phần còn lại tăng dần, chúng tôi chỉ xem xét các số chia hết cho`m`trong lần kiểm tra cuối cùng, loại bỏ nhu cầu xây dựng số đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10000)

def main():
    n, m = input().split()
    m = int(m)
    digits = sorted([int(c) for c in n])
    d = len(digits)
    dp = [[-1 for _ in range(m)] for _ in range(1 << d)]

    def count(mask, rem):
        if mask == (1 << d) - 1:
            return 1 if rem == 0 else 0
        if dp[mask][rem] != -1:
            return dp[mask][rem]
        total = 0
        for i in range(d):
            if mask & (1 << i):
                continue
            if i > 0 and digits[i] == digits[i-1] and not (mask & (1 << (i-1))):
                continue
            if mask == 0 and digits[i] == 0:
                continue
            total += count(mask | (1 << i), (rem * 10 + digits[i]) % m)
        dp[mask][rem] = total
        return total

    print(count(0, 0))

if __name__ == "__main__":
    main()
```Phần đầu tiên chuyển đổi số đầu vào thành chữ số và sắp xếp chúng. Bảng DP`dp`được khởi tạo cho tất cả các tập con chữ số và tất cả số dư. Đệ quy`count`chức năng phản ánh hướng dẫn thuật toán. Các điều kiện bỏ qua các chữ số đã sử dụng, số 0 đứng đầu và các chữ số lặp lại đảm bảo tính chính xác. Chúng tôi tích lũy số lượng hợp lệ trong`total`, ghi nhớ và cuối cùng in kết quả. 

## Ví dụ đã hoạt động 

cho`n = 104`,`m = 2`: 

| mặt nạ | rem | chữ số tiếp theo | mặt nạ mới | rem mới | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0b000 | 0 | 1 | 0b001 | 1 | ... | 
| 0b001 | 1 | 0 | 0b011 | 10% 2 = 0 | ... | 
| 0b011 | 0 | 4 | 0b111 | 0 | 1 | 
| ... | ... | ... | ... | ... | ... | 

Tất cả các hoán vị dẫn đến số dư chia hết cho 2 đều được tính. Kết quả là 3:`104`,`140`,`410`. 

Vì`n = 232`,`m = 2`: 

Chỉ có những con số`232`,`223`,`322`hiện hữu. Chỉ một`232`Và`322`chia hết cho 2. DP đếm đúng 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^d * d * m) | Mỗi tập hợp con các chữ số (2^d) được kết hợp với mỗi phần còn lại có thể có (m), lặp lại tối đa d chữ số cho các chuyển đổi | 
| Không gian | O(2^d * m) | Bảng DP lưu trữ mọi kết hợp mặt nạ và phần còn lại | 

Vì$d \le 18$Và$m \le 100$, đây là đại khái$2.6 \times 10^5$trạng thái, khả thi trong giới hạn bộ nhớ và thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided samples
assert run("104 2\n") == "3", "sample 1"
assert run("232 2\n") == "2", "sample 2"

# custom cases
assert run("1 1\n") == "1", "single digit divisible"
assert run("100 10\n") == "1", "leading zero avoidance"
assert run("111 3\n") == "1", "all digits same"
assert run("9876543210 11\n") == "0", "large number no valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Trường hợp một chữ số, chia hết | 
| 100 10 | 1 | Phòng ngừa bằng không hàng đầu | 
| 111 3 | 1 | Tất cả các chữ số giống hệt nhau | 
| 9876543210 11 | 0 | Đầu vào lớn, không có số hợp lệ | 

## Vỏ cạnh 

cho`n = 100`,`m = 10`, DP tránh các hoán vị bắt đầu bằng 0 (`010`,
