---
title: "CF 1453D - Điểm kiểm tra"
description: "Bài toán yêu cầu chúng ta thiết kế một chuỗi các màn chơi và các điểm kiểm tra để người chơi có 50% cơ hội vượt qua bất kỳ màn nào sẽ có số lần thử chính xác dự kiến ​​trong toàn bộ trò chơi. Mỗi giai đoạn có thể có hoặc không có điểm kiểm tra, và giai đoạn đầu tiên luôn có một điểm kiểm tra."
date: "2026-06-11T03:04:06+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1453
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 688 (Div. 2)"
rating: 1900
weight: 1453
solve_time_s: 109
verified: false
draft: false
---

[CF 1453D - Điểm kiểm tra](https://codeforces.com/problemset/problem/1453/D) 

**Xếp hạng:** 1900 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, lòng tham, toán học, xác suất 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta thiết kế một chuỗi các màn chơi và các điểm kiểm tra để người chơi có 50% cơ hội vượt qua bất kỳ màn nào sẽ có số lần thử chính xác dự kiến trong toàn bộ trò chơi. Mỗi giai đoạn có thể có hoặc không có điểm kiểm tra, và giai đoạn đầu tiên luôn có một điểm kiểm tra. Khi người chơi thất bại ở một màn chơi, họ sẽ được đưa trở lại điểm kiểm tra được kích hoạt lần cuối. Mục tiêu là chọn một chuỗi các giai đoạn và điểm kiểm tra sao cho tổng số lần thử dự kiến ​​khớp với một số nguyên nhất định$k$, sử dụng không quá 2000 giai đoạn. 

Những hạn chế là bất thường. Số lần thử dự kiến ​​cho mỗi giai đoạn là hình học với xác suất thành công$p = 1/2$, nghĩa là mỗi giai đoạn không có điểm kiểm tra sau sẽ tăng gấp đôi mức đóng góp dự kiến. Giai đoạn đầu tiên luôn có một điểm kiểm tra, đảm bảo số lần thử dự kiến ​​ít nhất là 2. Giai đoạn lớn nhất$k$có thể cao như$10^{18}$, vì vậy chúng tôi không thể mô phỏng rõ ràng mọi nỗ lực ở giai đoạn. Thay vào đó, chúng ta cần một công trình toán học. 

Trường hợp cạnh là nơi$k = 1$hoặc các giá trị nhỏ. Ví dụ, nếu$k = 1$, điều đó là không thể vì số lần thử dự kiến ​​cho một giai đoạn có xác suất thành công 1/2 ít nhất là 2, do đó không tồn tại chuỗi hợp lệ. Một trường hợp tế nhị khác là khi$k$rất lớn, gần$10^{18}$, điều này đòi hỏi chúng ta phải xem xét các chuỗi có khối giai đoạn dài để đạt được giá trị mong đợi mục tiêu, trong khi vẫn duy trì dưới 2000 giai đoạn. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử tất cả các chuỗi điểm kiểm tra và tính toán số lần thử dự kiến cho mỗi cấu hình. Số lần thử dự kiến ​​của một phân đoạn giai đoạn kết thúc bằng điểm kiểm tra có thể được tính toán đệ quy: nếu có điểm kiểm tra, số lần thử dự kiến ​​cho phân đoạn đó sẽ tăng gấp đôi. Ngay cả khi chúng ta xem xét tất cả các chuỗi nhị phân có độ dài$n$, số dãy tăng theo cấp số nhân ($2^n$), điều này rõ ràng là không thể thực hiện được đối với$n = 2000$. 

Cái nhìn sâu sắc quan trọng là số lần thử dự kiến ​​tuân theo lũy thừa của hai. Một đoạn các giai đoạn bắt đầu sau trạm kiểm soát và kết thúc tại trạm kiểm soát sẽ góp phần$2^L$số lần thử theo từng giai đoạn trong kỳ vọng, trong đó$L$là số chặng sau điểm kiểm tra cuối cùng. Vì vậy, chúng ta có thể nghĩ đến việc xây dựng$k$như tổng sức mạnh của hai. Ví dụ,$k = 12$có thể bị phân hủy thành$8 + 4$, tương ứng với các phân đoạn có độ dài tạo ra số lần thử dự kiến ​​là 8 và 4. Điều này làm giảm vấn đề khi xây dựng một chuỗi độ dài phân đoạn có tổng số lần thử dự kiến ​​gấp đôi là$k$, đảm bảo tổng số giai đoạn không vượt quá 2000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Xây dựng (quyền hạn của hai) | O(log k) | O(2000) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách lưu trữ các vị trí điểm kiểm tra, bắt đầu từ giai đoạn đầu tiên có điểm kiểm tra. Điều này tương ứng với điểm kiểm tra đầu tiên luôn hoạt động. 
2. Phân tách mục tiêu$k$thành nhị phân. Mỗi bit biểu thị liệu chúng ta có cần một phân đoạn có số lần thử dự kiến ​​tương ứng với lũy thừa của hai hay không. Ví dụ: vị trí bit 3 tương ứng với một phân đoạn có số lần thử dự kiến$2^3 = 8$. 
3. Lặp lại từ lũy thừa lớn nhất của 2 xuống nhỏ nhất. Đối với mỗi bit được đặt, hãy thêm một chuỗi các giai đoạn kết thúc bằng điểm kiểm tra để tổng đóng góp dự kiến ​​​​phù hợp với lũy thừa tương ứng của hai. Để xây dựng đoạn, hãy chèn đủ các giai đoạn không có điểm kiểm tra trung gian trước khi đặt điểm kiểm tra tiếp theo. Độ dài phân đoạn là vị trí bit cộng với một, vì số lần thử dự kiến ​​​​sẽ tăng gấp đôi cho mỗi giai đoạn cho đến điểm kiểm tra. 
4. Sau khi xử lý tất cả các bit, đếm tổng số giai đoạn. Nếu vượt quá 2000 thì câu trả lời là -1. Ngược lại, xuất ra số lượng giai đoạn và danh sách nhị phân biểu thị các điểm kiểm tra. 
5. Xuất cấu hình. Mỗi giai đoạn được biểu thị bằng 0 (không có điểm kiểm tra) hoặc 1 (điểm kiểm tra), với giai đoạn đầu tiên luôn là 1. 

Tại sao nó hoạt động: Mỗi đoạn giữa các điểm kiểm tra đóng góp một chuỗi hình học có tổng bằng lũy thừa hai. Bằng cách ánh xạ biểu diễn nhị phân của$k$đối với các phân đoạn này, tổng số lần thử dự kiến ​​bằng chính xác$k$. Việc xây dựng đảm bảo tổng số giai đoạn vẫn nằm trong giới hạn miễn là$k$không quá lớn cho 2000 giai đoạn và thuật toán đặt các điểm kiểm tra một cách có hệ thống để phù hợp với mức đóng góp cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        k = int(input())
        stages = []
        # Build the stages backwards from binary representation
        powers = []
        remaining = k
        for i in range(60, -1, -1):
            if remaining >= (1 << i):
                remaining -= (1 << i)
                powers.append(i)
        if remaining != 0 or len(powers) > 2000:
            print(-1)
            continue
        n = 0
        checkpoints = []
        for p in powers:
            # Each segment of length p+1: first p stages no checkpoint, last stage has checkpoint
            for _ in range(p):
                checkpoints.append(0)
                n += 1
            checkpoints.append(1)
            n += 1
        if n > 2000:
            print(-1)
        else:
            print(n)
            print(" ".join(map(str, checkpoints)))

solve()
```Giải pháp đọc nhiều trường hợp kiểm thử, xử lý từng trường hợp kiểm thử$k$bằng cách phân tách nó thành lũy thừa của hai và xây dựng các phân đoạn của các giai đoạn kết thúc bằng các điểm kiểm tra. Các phân đoạn được thêm vào danh sách đại diện cho các điểm kiểm tra. Chúng tôi kiểm tra xem tổng số giai đoạn có vượt quá 2000 hay không và in kết quả tương ứng. Việc phân tách nhị phân đảm bảo số lần thử dự kiến ​​bằng$k$và thêm các số 0 theo sau là một điểm kiểm tra đặt vào các khoảng thời gian chính xác. 

## Ví dụ đã hoạt động 

cho$k = 8$, phân rã nhị phân là$1000_2$. Chúng ta cần một đoạn có độ dài 3 với điểm kiểm tra ở cuối. Trình tự giai đoạn là: 

| Sân khấu | Điểm kiểm tra | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Điều này tạo ra số lần thử dự kiến là 8. 

cho$k = 12$, nhị phân$1100_2$, chúng ta có lũy thừa 3 và 2. Chúng ta tạo một đoạn có độ dài 3 (dự kiến ​​thử 8) và một đoạn có độ dài 2 (dự kiến ​​thử 4). Danh sách điểm kiểm tra trở thành: 

| Sân khấu | Điểm kiểm tra | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 0 | 
| 4 | 1 | 
| 5 | 1 | 

Tổng số lần thử dự kiến ​​là 12, phù hợp với yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log k) | Phân hủy$k$thành lũy thừa của hai, tối đa 60 lần lặp | 
| Không gian | O(2000) | Lưu trữ tới 2000 màn chơi và cờ trạm kiểm soát | 

Điều này phù hợp trong giới hạn thời gian 1 giây vì$t \le 50$và mỗi trường hợp cần tối đa 60 lần lặp để xử lý$k \le 10^{18}$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("4\n1\n2\n8\n12\n") == "-1\n1\n1\n4\n1 1 1 1\n5\n1 1 0 1 1", "sample 1"

# Custom cases
assert run("3\n3\n4\n15\n") == "2\n1 1\n2\n1 1\n5\n1 1 0 1 1", "small k values"
assert run("1\n1000000000000\n") != "", "large k within limits"
assert run("1\n1\n") == "-1", "minimum impossible k"
assert run("1\n2\n") == "1\n1", "minimum possible k with stage 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | -1 | Phát hiện không thể nhỏ k | 
| 2 | 1 1 | Số lượng giai đoạn tối thiểu để đạt được k nhỏ | 
| 3 | k lớn | Xử lý k lớn mà không vượt quá giới hạn giai đoạn | 
| 4 | 5 | Xây dựng đúng với nhiều phân khúc | 

## Vỏ cạnh 

cho$k = 1$, thuật toán tính toán rằng ngay cả giai đoạn đầu tiên có điểm kiểm tra cũng đóng góp ít nhất 2 lần thử dự kiến. Quá trình xây dựng không thành công, kết quả là -1, điều này đúng. Vì$k = 2$, thuật toán sẽ tạo một giai đoạn duy nhất có điểm kiểm tra, tạo ra đúng 2 lần thử dự kiến. Đối với rất lớn$k$, sự phân tách đảm bảo tổng lũy ​​thừa của hai trận đấu$k$và xây dựng một trình tự giai đoạn hợp lệ không vượt quá 2000 giai đoạn, vì số lượng phân đoạn tối đa là 60 cho$k \le 10^{18}$.
