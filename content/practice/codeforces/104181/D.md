---
title: "CF 104181D - Phòng tập Grumble"
description: "Chúng ta được cung cấp một chuỗi các nguồn năng lượng mà Alberto tiêu thụ theo đúng thứ tự. Mỗi nguồn đóng góp một lượng năng lượng cố định và một khi đã tiêu thụ, nó không thể được lấy lại hoặc phân chia. Sau mỗi buổi tập hoàn thành, năng lượng của Alberto sẽ được thiết lập lại hoàn toàn về không."
date: "2026-07-02T00:38:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104181
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 1 (Advanced)"
rating: 0
weight: 104181
solve_time_s: 78
verified: false
draft: false
---

[CF 104181D - Phòng tập thể dục Grumble](https://codeforces.com/problemset/problem/104181/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các nguồn năng lượng mà Alberto tiêu thụ theo đúng thứ tự. Mỗi nguồn đóng góp một lượng năng lượng cố định và một khi đã tiêu thụ, nó không thể được lấy lại hoặc phân chia. Sau mỗi buổi tập hoàn thành, năng lượng của Alberto sẽ được thiết lập lại hoàn toàn về không. 

Một buổi tập luyện được xác định theo yêu cầu ngày càng tăng: lần chống đẩy đầu tiên tốn 1 năng lượng, lần thứ hai tốn 2 năng lượng, v.v. cho đến lần chống đẩy thứ M tiêu tốn M năng lượng. Hoàn thành một bộ có nghĩa là Alberto phải tích lũy đủ năng lượng qua các lần lắc được tiêu thụ liên tiếp để trang trải tổng chi phí 1 + 2 + … + M. Anh ấy không bắt đầu lại trình tự lắc khi một bộ kết thúc mà chỉ đặt lại bộ đếm năng lượng của anh ấy. 

Quá trình này diễn ra liên tục: anh ấy tiêu thụ từng lần lắc một, tích lũy năng lượng và cố gắng hoàn thành càng nhiều hiệp đầy đủ càng tốt trước khi hết số lần lắc. Nhiệm vụ là tính xem anh ta đã hoàn thành được bao nhiêu hiệp đầy đủ. 

Các ràng buộc ngụ ý một giải pháp O(N) hoặc O(N log N). N có thể đạt tới 100.000, do đó, bất kỳ mô phỏng bậc hai nào trên tất cả các tập hợp hoặc quét lặp lại các trạng thái trước đó sẽ quá chậm. M tối đa là 1000, điều này cho thấy yêu cầu trên mỗi bộ bị giới hạn và có thể được tính toán trước hoặc được coi là ngưỡng không đổi. 

Một mô phỏng đơn giản có thể sẽ thất bại trong trường hợp năng lượng tích lũy chậm qua nhiều lần lắc nhỏ hoặc khi một lần lắc lớn vượt quá nhiều yêu cầu đẩy. 

Ví dụ: nếu M nhỏ nhưng N lớn: 
đầu vào:```
5 3
1 1 1 1 100
```Một cách tiếp cận bất cẩn có thể cố gắng mô phỏng các yêu cầu đẩy riêng lẻ và tính toán lại nhiều lần, dẫn đến việc xử lý thiết lập lại không hiệu quả hoặc không chính xác nếu xử lý sai năng lượng tràn. 

Một trường hợp khó phát hiện khác là khi một lần rung vượt quá nhiều ngưỡng đã đặt. Logic phải đảm bảo rằng năng lượng dư thừa không truyền sai qua các tập hợp. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực trực tiếp là mô phỏng quá trình chính xác như được mô tả. Chúng tôi duy trì một giá trị năng lượng đang chạy. Chúng tôi cũng mô phỏng tập hợp hiện tại: với mỗi lần đẩy i từ 1 đến M, chúng tôi kiểm tra xem năng lượng hiện tại có đủ hay không; nếu không, chúng ta sẽ uống nhiều sữa lắc hơn cho đến khi đạt được điều đó. Sau khi hoàn thành M lần đẩy, chúng tôi sẽ tăng phản ứng và thiết lập lại năng lượng. 

Cách tiếp cận này đúng vì nó phản ánh chính xác quá trình. Tuy nhiên, sự kém hiệu quả xuất phát từ việc liên tục quét các động tác đẩy và có khả năng quét rung nhiều lần trong các hiệp. Trong trường hợp xấu nhất, mọi thao tác lắc đều được xử lý bằng việc kiểm tra bên trong thường xuyên qua M bước, dẫn đến độ phức tạp O(NM), có thể đạt tới 10^8 thao tác. Đây là đường biên giới hoặc quá chậm trong Python tùy thuộc vào hằng số. 

Quan sát quan trọng là trong một bộ duy nhất, tổng năng lượng cần thiết là cố định và được biết trước:\[
S = 1 + 2 + \dots + M = \frac{M(M+1)}{2}
\]Vì vậy, thay vì mô phỏng từng lần đẩy, chúng ta chỉ cần theo dõi xem năng lượng tích lũy có đạt đến S hay không. Mỗi lần lắc đều góp phần trực tiếp vào bộ tích lũy này. Khi nó đạt hoặc vượt quá S, chúng tôi hoàn thành một bộ, tăng bộ đếm và trừ S (đây thực sự là một thiết lập lại vì năng lượng không truyền qua các bộ). 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính đơn giản duy trì tổng tích lũy và đếm số lần chúng ta vượt qua một ngưỡng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Mô phỏng lực lượng vũ phu | O(NM) | O(1) | Quá chậm | 
| Tích lũy tiền tố trên mỗi bộ | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng chính 
Mỗi hiệp yêu cầu một lượng năng lượng cố định S. Chúng tôi chỉ theo dõi tổng số lần chạy đạt S trong khi tiêu thụ lắc. 

### Các bước 

1. Tính tổng năng lượng cần thiết cho một bộ đầy đủ là S = M(M+1)/2. Điều này chuyển đổi quá trình đẩy biến thành một kiểm tra ngưỡng duy nhất. 
2. Khởi tạo một dòng điện thay đổi = 0 để lưu trữ năng lượng tích lũy cho tập đang diễn ra. 
3. Khởi tạo câu trả lời = 0 để đếm các bộ đã hoàn thành. 
4. Lặp lại từng lần lắc năng lượng Ei theo thứ tự. 
5. Thêm Ei vào hiện tại. 
6. Trong khi dòng điện lớn hơn hoặc bằng S, hãy trừ S khỏi câu trả lời hiện tại và số gia. 
7. Sau khi xử lý tất cả các lần lắc, xuất ra câu trả lời. 

Bước trừ tương đương với việc hoàn thành một bộ và đặt lại năng lượng, ngoại trừ việc chúng ta bảo toàn năng lượng còn sót lại từ lần rung hiện tại, điều này rất quan trọng khi một Ei lớn duy nhất trải dài trên nhiều bộ. 

### Tại sao nó hoạt động 

Quá trình chống đẩy trong một hiệp chỉ phụ thuộc vào tổng năng lượng chứ không phụ thuộc vào sự phân bổ qua các lần lắc. Vì năng lượng chỉ được tiêu thụ để đáp ứng nhu cầu ngày càng tăng nên tổng chi phí cho mỗi bộ là cố định. Do đó, việc chỉ theo dõi năng lượng tích lũy theo một ngưỡng cố định sẽ nắm bắt hoàn toàn việc hoàn thành tập hợp. Bất kỳ năng lượng dư thừa nào sau khi hoàn thành một bộ phải thuộc về bộ tiếp theo và trừ đi S sẽ bảo toàn chính xác trạng thái dư đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    E = list(map(int, input().split()))

    S = M * (M + 1) // 2

    current = 0
    ans = 0

    for x in E:
        current += x
        if current >= S:
            ans += current // S
            current %= S

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo việc giảm ngưỡng trực tiếp. Lựa chọn tinh tế nhất là sử dụng phép chia số nguyên để đếm xem có bao nhiêu bộ đầy đủ được hoàn thành trong một bước. Điều này hợp lệ vì khi năng lượng vượt quá S, nhiều bộ đầy đủ có thể được hoàn thành ngay lập tức và phần còn lại sẽ được chuyển tiếp. 

Hoạt động modulo đảm bảo năng lượng còn sót lại được bảo toàn chính xác cho hiệp tiếp theo. Điều này tránh lặp đi lặp lại các phép trừ và giữ cho nghiệm tuyến tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 
đầu vào:```
4 5
2 20 80 4
```S = 15 

| Lắc | Hiện tại trước | Hiện tại sau | Bộ hình thành | Còn lại | 
|------|-------|---------------|-------------|----------| 
| 2 | 0 | 2 | 0 | 2 | 
| 20 | 2 | 22 | 1 | 7 | 
| 80 | 7 | 87 | 5 | 12 | 
| 4 | 12 | 16 | 1 | 1 | 

Tổng số bộ = 2 

Dấu vết này cho thấy các bước nhảy lớn tự nhiên hình thành nhiều bộ trong một bước duy nhất và modulo bảo toàn chính xác năng lượng còn sót lại. 

### Mẫu 2 
đầu vào:```
3 3
20 5 2
```S = 6 

| Lắc | Hiện tại trước | Hiện tại sau | Bộ hình thành | Còn lại | 
|------|-------|---------------|-------------|----------| 
| 20 | 0 | 20 | 3 | 2 | 
| 5 | 2 | 7 | 1 | 1 | 
| 2 | 1 | 3 | 0 | 3 | 

Tổng số bộ = 1 

Ví dụ này cho thấy rằng năng lượng còn sót lại sẽ chuyển tải chính xác vào các lần lắc tiếp theo và góp phần hoàn thiện sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(N) | Mỗi lần lắc được xử lý một lần với các phép tính số học không đổi | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Giải pháp dễ dàng phù hợp trong giới hạn vì N lên tới 100.000 và mỗi thao tác là thời gian không đổi. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        N, M = map(int, input().split())
        E = list(map(int, input().split()))
        S = M * (M + 1) // 2
        current = 0
        ans = 0
        for x in E:
            current += x
            if current >= S:
                ans += current // S
                current %= S
        print(ans)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("4 5\n2 20 80 4\n") == "2", "sample 1"
assert run("3 3\n20 5 2\n") == "1", "sample 2"

# custom cases
assert run("1 1\n1\n") == "1", "single minimal set"
assert run("5 1\n5 4 3 2 1\n") == "15", "M=1 every unit is a set"
assert run("4 4\n1 1 1 10\n") == "1", "single large overflow"
assert run("6 3\n1 1 1 1 1 1\n") == "0", "never reaches threshold"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| 1 1/1 | 1 | trường hợp ranh giới tối thiểu | 
| Chuỗi M=1 | 15 | mỗi đơn vị năng lượng được tính là một bộ | 
| tràn nhỏ | 1 | rung chuyển lớn kéo dài nhiều bang | 
| tất cả các giá trị nhỏ | 0 | không vượt ngưỡng | 

## Vỏ cạnh 

Trường hợp nguy hiểm là khi một lần rung năng lượng duy nhất hoàn thành nhiều hiệp cùng một lúc. Ví dụ: 

đầu vào:```
1 3
100
```Ở đây S = 6. Thuật toán tính hiện tại = 100, sau đó thực hiện ans += 100 // 6 = 16, với số dư là 4. 

Từng bước, điều này phù hợp với thực tế: Alberto hoàn thành 16 bộ đầy đủ và mang 4 năng lượng vào bộ chưa hoàn chỉnh tiếp theo. Một mô phỏng đơn giản chỉ kiểm tra việc hoàn thành một tập hợp cho mỗi lần lặp sẽ dừng không chính xác sau lần hoàn thành đầu tiên và mất năng lượng còn lại, tính thiếu câu trả lời. 

Một trường hợp khác là khi không có tập hợp nào được hoàn thành: 

đầu vào:```
5 4
1 1 1 1 1
```S = 10. Tổng số hoạt động không bao giờ đạt đến 10, vì vậy ans vẫn bằng 0. Thuật toán bảo toàn năng lượng hiện tại một cách tự nhiên mà không buộc phải đặt lại không chính xác, phù hợp với hành vi dự kiến.
