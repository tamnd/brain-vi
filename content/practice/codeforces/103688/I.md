---
title: "CF 103688I - Mảng tổng bằng nhau"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu mảng số nguyên dương có thứ tự khác nhau có tổng bằng một số $k$ cho trước. Thứ tự rất quan trọng, vì vậy $[1,2]$ và $[2,1]$ được coi là khác nhau, mặc dù chúng có cùng số tiền."
date: "2026-07-02T20:53:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103688
codeforces_index: "I"
codeforces_contest_name: "The 17th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 103688
solve_time_s: 44
verified: true
draft: false
---

[CF 103688I - Mảng tổng bằng nhau](https://codeforces.com/problemset/problem/103688/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu mảng số nguyên dương có thứ tự khác nhau có tổng bằng một số cho trước$k$. Thứ tự rất quan trọng, vì vậy$[1,2]$Và$[2,1]$được coi là khác nhau mặc dù chúng có cùng số tiền. Mỗi phần tử phải có ít nhất 1 và mảng có thể có độ dài bất kỳ miễn là tổng của tất cả các phần tử bằng chính xác$k$. 

Điều này tương đương với việc hỏi chúng ta có thể phá vỡ bao nhiêu cách$k$thành một chuỗi các bước dương trong đó mỗi bước đóng góp một số nguyên dương và các thứ tự bước khác nhau tạo ra các kết quả khác nhau. Ví dụ, khi$k = 3$, chúng ta có thể lấy ba số 1 hoặc số 1 theo sau là số 2 hoặc số 2 theo sau là số 1 hoặc số 3. 

Ràng buộc$k \le 20$là cực kỳ nhỏ. Ngay cả khi chúng tôi cố gắng liệt kê tất cả các khả năng, sự tăng trưởng theo cấp số nhân vẫn có thể kiểm soát được. Một đệ quy ngây thơ phân nhánh trên tất cả các phần tử đầu tiên có thể vẫn sẽ kết thúc thoải mái trong giới hạn cho$k = 20$, nhưng nó sẽ lặp lại rất nhiều bài toán con chồng chéo, điều này gợi ý về lập trình động. 

Một trường hợp cạnh tinh tế là$k = 1$. Có chính xác một mảng$[1]$. Bất kỳ phương pháp nào giả định nhầm ít nhất hai phần tử hoặc cố gắng phân chia thêm sẽ tạo ra số 0 hoặc số âm không chính xác. Một trường hợp đặc biệt khác là xử lý các hoán vị không chính xác: ví dụ: ai đó có thể giả định sai$[1,2]$Và$[2,1]$phải giống nhau, điều này sẽ dẫn đến việc đếm các phân vùng số nguyên thay vì các thành phần. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tạo ra tất cả các mảng có tổng là$k$. Ở mỗi bước, chúng ta chọn phần tử tiếp theo của mảng từ 1 đến tổng còn lại. Nếu chúng tôi hiện đang xây dựng một tiền tố có tổng giá trị là$s$, tổng còn lại là$k - s$, sau đó chúng ta phân nhánh thành tất cả các lựa chọn của phần tử tiếp theo$x$Ở đâu$1 \le x \le k - s$. Chúng tôi tiếp tục cho đến khi tổng còn lại bằng 0, tương ứng với một mảng đầy đủ hợp lệ. 

Điều này hoạt động chính xác vì mọi mảng hợp lệ đều tương ứng với chính xác một đường dẫn trong cây đệ quy này. Tuy nhiên, số lượng các con đường như vậy tăng theo cấp số nhân. Vì$k = 20$, số tác phẩm là$2^{19} = 524288$, vẫn có thể quản lý được, nhưng đệ quy sẽ tính toán lại các trạng thái hậu tố giống hệt nhau nhiều lần. 

Nhận xét quan trọng là vấn đề chỉ phụ thuộc vào số tiền còn lại. Nếu chúng ta định nghĩa$dp[s]$bằng số lượng mảng có tổng bằng$s$, sau đó từ trạng thái$s$, chúng ta thử chọn phần tử đầu tiên$x$, và giảm vấn đề xuống$s - x$. Điều này có nghĩa là mọi trạng thái chỉ phụ thuộc vào các trạng thái nhỏ hơn và nhiều tiền tố khác nhau dẫn đến tổng còn lại như nhau. Sự chồng chéo đó làm cho việc ghi nhớ hoặc DP từ dưới lên trở nên tự nhiên. 

Chúng ta cũng có thể thấy sự tái diễn trực tiếp:$dp[0] = 1$, và cho$s > 0$, chúng ta tính tổng tất cả các phần tử đầu tiên có thể có:$$dp[s] = \sum_{x=1}^{s} dp[s-x]$$Đây là một tác phẩm cổ điển DP. Nó cũng đơn giản hóa thành một danh tính nổi tiếng trong đó$dp[s] = 2^{s-1}$vì$s \ge 1$, nhưng vì$k$rất nhỏ nên việc lấy DP là đủ và an toàn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force |$O(2^k)$|$O(k)$| Chấp nhận được nhưng dư thừa | 
| DP trên tổng trạng thái |$O(k^2)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Định nghĩa một mảng`dp`Ở đâu`dp[i]`đại diện cho số mảng có thứ tự hợp lệ có tổng bằng`i`. Điều này chuyển vấn đề từ việc xây dựng chuỗi sang cách đếm các cách để đạt được tổng mục tiêu. 
2. Khởi tạo`dp[0] = 1`. Điều này tương ứng với cấu trúc trống, là cách duy nhất để đạt được tổng bằng 0 mà không cần chọn bất kỳ phần tử nào. Trường hợp cơ bản này đảm bảo rằng các chuyển đổi sau này có điểm bắt đầu hợp lệ. 
3. Với mỗi giá trị`i`từ 1 đến`k`, tính toán`dp[i]`bằng cách xem xét tất cả các phần tử đầu tiên có thể có của một mảng có tổng bằng`i`. 
4. Đối với cố định`i`, hãy thử mọi phần tử đầu tiên có thể`x`từ 1 đến`i`. Sau khi chọn`x`, số tiền còn lại trở thành`i - x`, vì vậy chúng tôi thêm`dp[i - x]`ĐẾN`dp[i]`. Điều này giải thích chính xác cho tất cả các mảng có phần tử đầu tiên là`x`. 
5. Sau khi điền vào bảng DP tới`k`, đầu ra`dp[k]`như câu trả lời cuối cùng. 

Tại sao nó hoạt động: mỗi mảng hợp lệ tính tổng thành`i`có phần tử đầu tiên duy nhất và việc loại bỏ phần tử đầu tiên đó sẽ để lại một mảng hợp lệ có tổng giá trị nhỏ hơn. Điều này tạo ra sự tương ứng một-một giữa các mảng được tính trong`dp[i]`và các cặp gồm phần tử đầu tiên và hậu tố hợp lệ, bảo đảm không trùng lặp, thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input().strip())
    
    dp = [0] * (k + 1)
    dp[0] = 1

    for i in range(1, k + 1):
        total = 0
        for x in range(1, i + 1):
            total += dp[i - x]
        dp[i] = total

    print(dp[k])

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện phép lặp DP. Vòng lặp bên ngoài sửa tổng`i`, trong khi vòng lặp bên trong liệt kê tất cả các phần tử đầu tiên có thể có. Biến`total`tích lũy đóng góp từ tất cả các phần chia tách hợp lệ. Việc sử dụng bộ tích lũy riêng sẽ tránh việc ghi lặp lại vào mảng DP trong quá trình tính toán. 

Một lỗi triển khai phổ biến là quên khởi tạo`dp[0] = 1`. Không có nó, tất cả các trạng thái vẫn bằng 0 vì mọi chuyển đổi đều phụ thuộc vào các bài toán con nhỏ hơn cuối cùng sẽ đạt đến 0. Một điểm tinh tế khác là đảm bảo vòng lặp bên trong bắt đầu từ 1, vì số 0 không được phép làm phần tử mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1: k = 3 

Chúng tôi tính toán DP từng bước. 

| tôi | x (phần tử đầu tiên) | tôi - x | dp[i - x] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 1, 2 | 1, 0 | 1, 1 | 2 | 
| 3 | 1, 2, 3 | 2, 1, 0 | 2, 1, 1 | 4 | 

Vì$k = 3$, ta được 4 dãy:$[1,1,1]$,$[1,2]$,$[2,1]$, Và$[3]$. Bảng DP xác nhận rằng mỗi phân tách của phần tử đầu tiên đóng góp chính xác vào tổng số. 

### Ví dụ 2: k = 4 

| tôi | x | tôi - x | dp[i - x] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 1,2 | 1,0 | 1,1 | 2 | 
| 3 | 1,2,3 | 2,1,0 | 2,1,1 | 4 | 
| 4 | 1,2,3,4 | 3,2,1,0 | 4,2,1,1 | 8 | 

Điều này cho thấy mô hình nhân đôi đang nổi lên, phản ánh thực tế là mỗi tổng mới đưa ra một cấu trúc quyết định nhị phân mới về việc có nên mở rộng các thành phần trước đó hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k^2)$| Đối với mỗi khoản tiền$i$, chúng tôi lặp lại tất cả các phần tử đầu tiên có thể có$1 \dots i$| 
| Không gian |$O(k)$| Chúng tôi lưu trữ các giá trị DP cho tất cả các khoản tiền lên tới$k$| 

Với$k \le 20$, giải pháp chạy trong thời gian không đổi một cách hiệu quả. Ngay cả một phép đệ quy mũ đơn giản cũng sẽ vượt qua, nhưng DP giữ cho cấu trúc gọn gàng và tránh việc tính toán lại dư thừa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    k = int(inp.strip())
    dp = [0] * (k + 1)
    dp[0] = 1
    for i in range(1, k + 1):
        dp[i] = sum(dp[i - x] for x in range(1, i + 1))
    return str(dp[k])

# provided samples
assert run("1") == "1", "k=1"

# custom cases
assert run("2") == "2", "k=2"
assert run("3") == "4", "k=3"
assert run("4") == "8", "k=4"
assert run("5") == "16", "k=5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp cơ sở nhỏ nhất | 
| 2 | 2 | sự chia rẽ không tầm thường đầu tiên | 
| 4 | 8 | mô hình tăng trưởng theo cấp số nhân | 
| 5 | 16 | tính nhất quán của sự tái phát | 

## Vỏ cạnh 

cho$k = 1$, DP khởi tạo với`dp[0] = 1`, sau đó`dp[1] = dp[0] = 1`. Thuật toán tạo ra một mảng một cách chính xác$[1]$. Bất kỳ việc triển khai nào thiếu trường hợp cơ sở sẽ xuất ra số 0 không chính xác. 

Vì$k = 2$, việc tính toán diễn ra`dp[2] = dp[1] + dp[0] = 1 + 1 = 2`, tương ứng với$[1,1]$Và$[2]$. Điều này xác minh rằng cả hai trường hợp tiếp tục và chấm dứt đều được tính. 

Vì$k = 0$, mặc dù không có ràng buộc đầu vào, định nghĩa này bao hàm chính xác một cấu trúc trống và việc khởi tạo`dp[0] = 1`duy trì tính đúng đắn nếu được mở rộng.
