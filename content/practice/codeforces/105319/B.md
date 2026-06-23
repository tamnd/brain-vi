---
title: "CF 105319B - Dây bị đứt"
description: "Chúng ta được cung cấp một chuỗi gồm các chữ số thập phân. Thao tác duy nhất được phép là chọn một vị trí và di chuyển chữ số của nó lên hoặc xuống một bậc, nằm trong phạm vi từ 0 đến 9."
date: "2026-06-22T17:21:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105319
codeforces_index: "B"
codeforces_contest_name: "Tishreen Collegiate Programming Contest 2024"
rating: 0
weight: 105319
solve_time_s: 53
verified: true
draft: false
---

[CF 105319B - Chuỗi bị hỏng](https://codeforces.com/problemset/problem/105319/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi gồm các chữ số thập phân. Thao tác duy nhất được phép là chọn một vị trí và di chuyển chữ số của nó lên hoặc xuống một bước, nằm trong phạm vi từ 0 đến 9. Lặp lại thao tác này bao nhiêu lần, chúng ta muốn biến đổi chuỗi để nó đọc xuôi và ngược giống nhau. Chi phí là tổng số lần tăng hoặc giảm từng bước được thực hiện trên tất cả các vị trí và nhiệm vụ là giảm thiểu chi phí này. 

Cấu trúc của đầu vào quan trọng theo cách đơn giản nhưng nghiêm ngặt: mỗi vị trí chỉ tương tác với vị trí phản chiếu của nó từ đầu kia của chuỗi. Ràng buộc rằng tổng độ dài trên tất cả các trường hợp thử nghiệm tối đa là 1000 có nghĩa là bất kỳ giải pháp nào thậm chí thực hiện một lượng công việc bổ sung bậc hai cho mỗi trường hợp thử nghiệm đều đã an toàn, trong khi mọi thứ liên quan đến tính toán lại nặng nề lồng nhau vẫn sẽ vượt qua nhưng không cần thiết. 

Một sự hiểu lầm ngây thơ sẽ xảy ra nếu người ta cố gắng mô phỏng các hoạt động từng bước. Ví dụ: nếu chúng ta lấy một cặp như`1`Và`9`, người ta có thể tăng liên tục chữ số nhỏ hơn cho đến khi nó khớp với chữ số lớn hơn, tốn 8 thao tác. Điều đó đúng cục bộ, nhưng thực hiện tuần tự cho mọi mục tiêu trung gian có thể sẽ lãng phí thời gian suy luận và dẫn đến việc triển khai quá phức tạp. 

Một dạng sai sót tinh vi khác xuất hiện nếu người ta cho rằng cả hai chữ số phải được thay đổi theo hướng cố định một cách độc lập. Ví dụ: chuyển cả hai chữ số về 0 hoặc về 9 có thể trông đối xứng nhưng không phải lúc nào cũng tối ưu. Điểm gặp mặt tối ưu không cố định trên toàn cầu. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực xử lý từng vị trí một cách độc lập nhưng mô phỏng tất cả các cách có thể để làm cho các ký tự được phản chiếu trở nên bình đẳng. Đối với một cặp chữ số`a`Và`b`, người ta có thể thử chọn một chữ số mục tiêu`d`từ 0 đến 9 và tính chi phí chuyển đổi cả hai bên thành`d`. Điều đó mang lại một chi phí`|a - d| + |b - d|`, và lấy giá trị nhỏ nhất trên tất cả`d`tạo ra câu trả lời đúng. Điều này đúng vì mọi chữ số cuối cùng bằng nhau đều phải nằm ở đâu đó trên dòng số nguyên từ 0 đến 9. 

Tuy nhiên, điều này tạo ra một vòng lặp không cần thiết trên 10 mục tiêu có thể có cho mỗi cặp, dẫn đến khoảng`O(10n)`làm việc cho mỗi trường hợp thử nghiệm. Mặc dù vẫn tuyến tính nhưng nó che giấu thực tế là biểu thức có dạng đơn giản hóa dạng đóng. 

Quan sát quan trọng là hình học hơn là thủ tục. Trên trục số, việc di chuyển cả hai điểm đến một điểm gặp nhau là nhỏ nhất khi điểm gặp nhau đó nằm giữa chúng. Trong trường hợp đó, tổng khoảng cách sẽ giảm xuống khoảng cách trực tiếp giữa các điểm cuối. Vì vậy, thay vì tìm kiếm chữ số phù hợp, chúng ta có thể tính trực tiếp chi phí dưới dạng chênh lệch tuyệt đối giữa hai chữ số. 

Điều này làm giảm toàn bộ vấn đề thành tổng các khác biệt theo cặp này trên các vị trí được phản chiếu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả mục tiêu trên mỗi cặp) | O(n · 10) | O(1) | Đã chấp nhận | 
| Tối ưu (chênh lệch cặp trực tiếp) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và tính toán chi phí tối thiểu cho chuỗi đó. 

1. Đọc chuỗi và hiểu mỗi ký tự là một chữ số. Độ dài chuỗi xác định có bao nhiêu cặp được nhân đôi. 
2. Khởi tạo bộ tích lũy cho tổng chi phí bằng 0. Điều này sẽ thu thập các hoạt động tối thiểu cần thiết trên tất cả các cặp đối xứng. 
3. Đối với mỗi chỉ số`i`từ đầu chuỗi đến điểm giữa, ghép nó với chỉ mục`n - i - 1`. Điều này đảm bảo mọi vị trí được khớp chính xác một lần. 
4. Tính hiệu tuyệt đối giữa hai chữ số trong cặp. Giá trị này thể hiện số lượng tối thiểu các thao tác một bước cần thiết để làm cho chúng bằng nhau. 
5. Thêm số chênh lệch này vào bộ tích lũy. Mỗi cặp là độc lập nên các chi phí này cộng lại một cách trực tiếp. 
6. Sau khi xử lý tất cả các cặp, xuất ra tổng tích lũy. 

### Tại sao nó hoạt động 

Mỗi cặp được nhân đôi độc lập với tất cả các cặp khác vì các thao tác trên một vị trí không bao giờ ảnh hưởng đến vị trí khác. Đối với hai chữ số bất kỳ, chi phí tối thiểu để làm cho chúng bằng nhau đạt được bằng cách di chuyển cả hai về phía nhau dọc theo dòng số nguyên và tổng khoảng cách bao phủ trong quá trình đó chính xác là sự khác biệt tuyệt đối giữa chúng. Vì mọi palindrome hợp lệ phải thực thi sự bình đẳng trên tất cả các cặp được nhân đôi, mức tối ưu toàn cục là tổng chi phí của cặp tối ưu độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        n = len(s)
        ans = 0

        for i in range(n // 2):
            a = ord(s[i]) - ord('0')
            b = ord(s[n - i - 1]) - ord('0')
            ans += abs(a - b)

        print(ans)

if __name__ == "__main__":
    solve()
```Mã này tuân theo cấu trúc ghép các chỉ số đối xứng và tích lũy chênh lệch tuyệt đối. Việc chuyển đổi sử dụng`ord`tránh chi phí phân tích số nguyên lặp đi lặp lại, mặc dù trực tiếp`int()`cũng sẽ được chấp nhận với những hạn chế. 

Vòng lặp chỉ chạy đến`n // 2`, đảm bảo mỗi cặp được tính chính xác một lần. Không cần xử lý đặc biệt đối với các chuỗi có độ dài lẻ vì ký tự ở giữa đã tự thỏa mãn điều kiện palindrome và đóng góp chi phí bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`395`Chúng tôi ghép các vị trí`(0, 2)`chỉ vì ký tự ở giữa bị bỏ qua. 

| Bước | Trái | Đúng | Giá cặp | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 5 | 2 | 2 | 

Cách tốt nhất là làm cho cả hai chữ số bằng nhau và chi phí tối thiểu chính xác là khoảng cách giữa chúng. 

Điều này xác nhận rằng thuật toán không phụ thuộc vào việc chọn một chữ số mục tiêu chung một cách rõ ràng. 

### Ví dụ 2 

Chuỗi đầu vào:`1234`Chúng tôi tạo thành hai cặp:`(1,4)`Và`(2,3)`. 

| Bước | Trái | Đúng | Giá cặp | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 3 | 3 | 
| 2 | 2 | 3 | 1 | 4 | 

Kết quả chứng tỏ mỗi cặp đóng góp độc lập và không có sự tương tác giữa các vị trí khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập một lần trong một lần truyền qua nửa chuỗi | 
| Không gian | O(1) | Chỉ sử dụng tổng hiện có và một vài biến | 

Tổng độ dài của các trường hợp thử nghiệm tối đa là 1000, do đó việc quét tuyến tính trên mỗi trường hợp thử nghiệm có thể dễ dàng đủ nhanh. Ngay cả khi tất cả các chuỗi được xử lý riêng biệt thì tổng công việc vẫn có giới hạn và hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []
    
    def solve():
        t = int(input())
        for _ in range(t):
            s = input().strip()
            n = len(s)
            ans = 0
            for i in range(n // 2):
                a = ord(s[i]) - ord('0')
                b = ord(s[n - i - 1]) - ord('0')
                ans += abs(a - b)
            output.append(str(ans))
    
    solve()
    return "\n".join(output)

# provided samples (illustrative since original statement omits them)
assert run("3\n9\n12\n395\n") == "0\n1\n2", "basic sanity"

# custom cases
assert run("1\n0\n") == "0", "single digit"
assert run("1\n99\n") == "0", "already palindrome"
assert run("1\n19\n") == "8", "max distance pair"
assert run("1\n12321\n") == "0", "palindrome already"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0`| chuỗi có độ dài tối thiểu | 
|`99`|`0`| đã bằng nhau rồi | 
|`19`|`8`| khoảng cách chữ số tối đa | 
|`12321`|`0`| độ chính xác của palindrome có độ dài lẻ | 

## Vỏ cạnh 

Chuỗi ký tự đơn đã là một chuỗi palindrome vì không có ràng buộc phản chiếu nào cần đáp ứng. Thuật toán xử lý các cặp 0, do đó chi phí tích lũy vẫn bằng 0, phù hợp với kết quả mong đợi. 

Đối với một đầu vào như`19`, thuật toán ghép hai chữ số và tính`|1 - 9| = 8`. Mọi nỗ lực định tuyến qua các chữ số trung gian vẫn sẽ tích lũy chính xác tám bước đơn vị và tính toán trực tiếp phản ánh đường dẫn tối thiểu đó mà không cần mô phỏng. 

Đối với các chuỗi đã có palindromic như`1221`, mỗi cặp được nhân đôi bao gồm các chữ số giống hệt nhau, tạo ra sự đóng góp bằng 0 cho mỗi cặp. Thuật toán bảo toàn điều này một cách tự nhiên vì mỗi sai số tuyệt đối đều có giá trị bằng 0. 

Đối với các chuỗi có độ dài lẻ như`12321`, chữ số ở giữa không bao giờ được ghép với bất cứ thứ gì. Vòng lặp có chủ ý dừng ở một nửa chiều dài, đảm bảo tâm không ảnh hưởng đến kết quả và không xảy ra việc ghép nối không chính xác.
