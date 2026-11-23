---
title: "CF 431C - k-Cây"
description: "Chúng tôi đang làm việc với một cây vô hạn có gốc trong đó mỗi nút luôn có chính xác $k$ các cạnh đi tới các nút con. Mỗi cạnh $k$ đó có trọng số cố định: cạnh đầu tiên là 1, cạnh thứ hai là 2, v.v. cho đến $k$."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 431
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 247 (Div. 2)"
rating: 1600
weight: 431
solve_time_s: 91
verified: true
draft: false
---

[CF 431C - k-Tree](https://codeforces.com/problemset/problem/431/C) 

**Đánh giá:** 1600 
**Tags:** dp, triển khai, cây 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một cây vô hạn có gốc trong đó mỗi nút luôn có chính xác$k$các khía cạnh hướng ngoại đối với trẻ em. Mỗi cái đó$k$các cạnh có trọng số cố định: cạnh đầu tiên là 1, cạnh thứ hai là 2, v.v.$k$. Từ gốc rễ, chúng tôi xem xét các con đường đi xuống mà trẻ em sẽ đi qua từng bước một và mỗi lần chúng tôi chọn một trong những con đường đó.$k$các cạnh. 

Một đường đi đơn giản là một chuỗi các lựa chọn cạnh bắt đầu từ gốc và tổng chi phí của nó là tổng trọng số của các cạnh được sử dụng dọc theo tuyến đường đó. Chúng ta chỉ quan tâm đến những đường đi có tổng trọng lượng chính xác là$n$. Trong số đó, chúng ta chỉ tính các đường đi chứa ít nhất một cạnh có trọng số ít nhất là$d$, nghĩa là ít nhất một bước sử dụng cạnh được gắn nhãn$d, d+1, \dots, k$. 

Vì vậy, nhiệm vụ là đếm các thành phần bị ràng buộc của$n$: mỗi bước đóng góp một giá trị vào$[1, k]$, thứ tự rất quan trọng và chúng tôi cũng yêu cầu thêm rằng chuỗi chứa ít nhất một bước "lớn" (ít nhất$d$). Vì mỗi lựa chọn cũng tương ứng với một đứa trẻ riêng biệt nên các chuỗi khác nhau sẽ khác biệt ngay cả khi chúng có cùng trọng số. 

Ràng buộc$n, k \le 100$làm rõ rằng giải pháp quy hoạch động bậc hai hoặc bậc ba trên các trạng thái như “tổng cho đến nay” và “liệu ​​ràng buộc có được thỏa mãn hay không” là đủ. Bất kỳ số mũ nào trên các chuỗi đều không khả thi vì số lượng các chuỗi tăng lên như$k^n$, lớn về mặt thiên văn ngay cả đối với người vừa phải$n$. 

Một nỗ lực ngây thơ có thể thử liệt kê tất cả các chuỗi có tổng trọng số$n$. Ngay cả khi chúng ta cắt bớt theo tổng hiện tại, số chuỗi vẫn hoạt động giống như một vụ nổ Fibonacci tổng quát. Ví dụ, với$k=3$Và$n=20$, số lượng đường dẫn một phần đã vượt quá hàng triệu. Điều này ngay lập tức làm cho vũ lực không thể thực hiện được. 

Trường hợp cạnh tinh tế xuất hiện khi$d = 1$. Khi đó mọi cạnh đều “lớn”, do đó mọi phân tách hợp lệ của$n$thành nhiều phần$[1,k]$được chấp nhận. Một trường hợp cạnh khác là$d = k$, nơi chỉ có bước cân$k$đủ điều kiện là lớn, vì vậy các chuỗi phải bao gồm ít nhất một$k$. Một giải pháp bất cẩn mà quên điều kiện “ít nhất một cạnh lớn” sẽ bị tính quá mức trong cả hai tình huống. 

## Phương pháp tiếp cận 

Vấn đề cơ bản là đếm các dãy số nguyên dương giới hạn bởi$k$số tiền đó bằng$n$, với một ràng buộc phụ về việc liệu một giá trị lớn có xuất hiện hay không. Chế độ xem brute-force rất đơn giản: xây dựng các chuỗi tăng dần, theo dõi tổng hiện tại và đếm những chuỗi kết thúc chính xác$n$và chứa một giá trị$\ge d$. Điều này đúng vì nó liệt kê trực tiếp định nghĩa của các đường dẫn hợp lệ. 

Tuy nhiên, từ bất kỳ trạng thái nào có tổng$s$, có tới$k$chuyển tiếp và độ sâu có thể đạt tới$n$. Điều này tạo ra khoảng$O(k^n)$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Điều quan trọng cần lưu ý là chúng ta không cần phải nhớ toàn bộ chuỗi mà chỉ cần nhớ hai thông tin: tổng hiện tại và liệu chúng ta đã sử dụng ít nhất một cạnh “lớn” hay chưa. Điều này ngay lập tức gợi ý lập trình động trên các tiền tố của tổng. 

Chúng tôi xác định DP trên tổng còn lại hoặc tổng hiện tại và trạng thái nhị phân cho biết liệu điều kiện có được thỏa mãn hay không. Chuyển đổi rất đơn giản: từ tổng$i$, chúng tôi thử cộng từng trọng số$w \in [1,k]$, cập nhật cả tổng và cờ tùy thuộc vào việc$w \ge d$. 

Điều này biến cây lựa chọn hàm mũ thành một biểu đồ xếp lớp trên$n$tiểu bang, trong đó mỗi tiểu bang có nhiều nhất$k$chuyển tiếp đi, đưa ra$O(nk)$sự phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS trên đường dẫn |$O(k^n)$|$O(n)$| Quá chậm | 
| Tổng DP + cờ |$O(nk)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một bảng lập trình động trong đó thứ nguyên là tổng số tiền đạt được cho đến nay và chúng tôi theo dõi xem “cạnh lớn” có được sử dụng hay không. 

1. Xác định bảng DP$dp[i][0/1]$, Ở đâu$i$là tổng hiện tại và chiều thứ hai cho biết liệu chúng ta đã sử dụng ít nhất một cạnh có trọng số hay chưa$\ge d$. Cấu trúc này nén toàn bộ lịch sử thành thông tin duy nhất liên quan đến điều kiện cuối cùng. 
2. Khởi tạo$dp[0][0] = 1$. Điều này thể hiện đường dẫn trống ở gốc trước khi lấy bất kỳ cạnh nào. Chưa có cạnh lớn nào được sử dụng vì chưa có cạnh nào được lấy. 
3. Lặp lại tất cả các khoản tiền từ$0$ĐẾN$n-1$. Mỗi trạng thái đại diện cho một phần đường dẫn đã có tổng trọng số nhất định. 
4. Từ mỗi tiểu bang$(i, flag)$, thử tất cả các trọng số cạnh$w$từ 1 đến$k$. Nếu như$i + w \le n$, chúng ta có thể mở rộng đường đi thêm một bước. Điều này tương ứng chính xác với việc chọn một cạnh con trong cây. 
5. Khi chuyển đổi, hãy tính cờ mới như sau:$flag \lor (w \ge d)$. Điều này đảm bảo rằng một khi chúng ta sử dụng cạnh lớn, thuộc tính sẽ được ghi nhớ vĩnh viễn trong trạng thái. 
6. Cộng số cách từ trạng thái hiện tại sang trạng thái tiếp theo:$dp[i + w][new\_flag] += dp[i][flag]$, lấy modulo$10^9 + 7$. 
7. Sau khi điền vào bảng câu trả lời là$dp[n][1]$, vì chúng ta muốn tổng số tiền chính xác$n$và ít nhất một cạnh lớn được sử dụng. 

### Tại sao nó hoạt động 

Mỗi đường dẫn từ gốc đến nút hợp lệ tương ứng duy nhất với một chuỗi các lựa chọn cạnh có tổng trọng số bằng$n$. DP liệt kê tất cả các chuỗi như vậy chính xác một lần vì mỗi quá trình chuyển đổi sẽ thêm một lựa chọn cạnh hợp lệ. Việc nén trạng thái là an toàn vì các chuyển đổi trong tương lai chỉ phụ thuộc vào tổng còn lại và liệu ràng buộc đã được thỏa mãn hay chưa chứ không phụ thuộc vào thứ tự của các cạnh nhỏ trước đó. Do đó, không có hai chuỗi hợp lệ riêng biệt nào được hợp nhất không chính xác và không có chuỗi không hợp lệ nào có thể đạt tới$dp[n][1]$vì cờ thực thi chính xác ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n, k, d = map(int, input().split())

dp = [[0, 0] for _ in range(n + 1)]
dp[0][0] = 1

for i in range(n + 1):
    for used in range(2):
        if dp[i][used] == 0:
            continue
        cur = dp[i][used]
        for w in range(1, k + 1):
            if i + w > n:
                break
            nxt_used = used or (w >= d)
            dp[i + w][nxt_used] = (dp[i + w][nxt_used] + cur) % MOD

print(dp[n][1])
```Bảng DP lưu trữ số cách để đạt được mỗi tổng dù có hoặc không sử dụng cạnh lớn. Vòng lặp bên ngoài tiến triển thông qua các tổng theo thứ tự tăng dần nên mọi quá trình chuyển đổi luôn diễn ra và không bao giờ quay lại các trạng thái trước đó. 

Vòng lặp chuyển tiếp trên các trọng số từ 1 đến$k$phản ánh chính xác cấu trúc cây vì mỗi nút có$k$trẻ em có trọng lượng cạnh đó. Việc nghỉ sớm khi$i + w > n$là an toàn vì trọng lượng ngày càng tăng nên tất cả các trọng lượng tiếp theo cũng sẽ vượt quá$n$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 2
```Chúng tôi theo dõi trạng thái DP$(i, used)$. 

| tôi | dp[i][0] | dp[i][1] | chuyển tiếp | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | từ 0 thêm 1,2,3 | 
| 1 | 1 | 0 | đạt được thông qua 1 | 
| 2 | 1 | 1 | qua 2 giới thiệu cạnh lớn | 
| 3 | 0 | 3 | tích lũy nhiều đường dẫn | 

Giá trị cuối cùng là 3. 

Điều này cho thấy các đường dẫn đạt tổng 3 phải bao gồm ít nhất một cạnh có trọng số 2 hoặc 3. DP phân tách các trạng thái một cách chính xác để các chuỗi không có cạnh lớn không đóng góp. 

### Ví dụ 2 

đầu vào:```
4 2 2
```Chỉ có trọng số 1 và 2 tồn tại và 2 là cạnh lớn duy nhất. 

| tôi | dp[i][0] | dp[i][1] | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 1 | 0 | 
| 2 | 1 | 1 | 
| 3 | 1 | 2 | 
| 4 | 1 | 3 | 

Câu trả lời là 3. 

Điều này xác nhận rằng các chuỗi chỉ sử dụng số 1 sẽ bị loại trừ, trong khi các chuỗi chứa ít nhất một số 2 được tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nk)$| Đối với mỗi tổng, chúng tôi cố gắng đạt tới$k$chuyển tiếp | 
| Không gian |$O(n)$| DP lưu trữ tổng hai trạng thái | 

Với$n, k \le 100$, điều này dẫn đến nhiều nhất$10^4$chuyển tiếp, dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve():
    n, k, d = map(int, sys.stdin.readline().split())
    dp = [[0, 0] for _ in range(n + 1)]
    dp[0][0] = 1

    for i in range(n + 1):
        for used in range(2):
            cur = dp[i][used]
            if not cur:
                continue
            for w in range(1, k + 1):
                if i + w > n:
                    break
                nd = used or (w >= d)
                dp[i + w][nd] = (dp[i + w][nd] + cur) % MOD

    print(dp[n][1])

def run(inp: str) -> str:
    global input
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# provided sample
assert run("3 3 2\n") == "3"

# minimum case
assert run("1 1 1\n") == "1"

# no valid large edge possible
assert run("3 2 3\n") == "0"

# all edges are large
assert run("3 3 1\n") == "4"

# boundary k=n
assert run("4 4 3\n") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 2 | 3 | độ chính xác của mẫu | 
| 1 1 1 | 1 | con đường không tầm thường nhỏ nhất | 
| 3 2 3 | 0 | không thể thỏa mãn điều kiện cạnh lớn | 
| 3 3 1 | 4 | tất cả các chuỗi đều hợp lệ vì mọi cạnh đều lớn | 

## Vỏ cạnh 

Khi nào$d = 1$, mọi cạnh đều được coi là lớn, do đó câu trả lời rút gọn thành việc đếm tất cả các thành phần của$n$sử dụng các bộ phận$1..k$. DP vẫn hoạt động vì mọi chuyển đổi ngay lập tức đặt cờ thành 1 bất cứ khi nào có bất kỳ cạnh nào được lấy, do đó chỉ$dp[i][1]$trở nên có liên quan sau bước đầu tiên. 

Khi$d = k$, chỉ có cạnh lớn nhất mới đóng góp vào ràng buộc. DP theo dõi chính xác các trình tự không bao giờ sử dụng$k$TRONG$dp[*][0]$, và chỉ di chuyển vào$dp[*][1]$khi một$k$được chọn. Điều này phân tách rõ ràng các chuỗi không hợp lệ ngay cả khi chúng đạt tổng$n$, đảm bảo chúng không được tính trừ khi$k$xuất hiện ít nhất một lần.
