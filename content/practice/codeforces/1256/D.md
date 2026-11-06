---
problem: 1256D
contest_id: 1256
problem_index: D
name: "Binary String Minimizing"
contest_name: "Codeforces Round 598 (Div. 3)"
rating: 1500
tags: ["greedy"]
answer: passed_samples
verified: true
solve_time_s: 430
date: 2026-06-13
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d779c-d51c-83ec-92b9-0b69bdc465a4
---

#CF 1256D - Giảm thiểu chuỗi nhị phân 

**Đánh giá:** 1500 
**Thẻ:** tham lam 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 7 phút 10 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d779c-d51c-83ec-92b9-0b69bdc465a4 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân và ngân sách các giao dịch hoán đổi liền kề. Mỗi trao đổi trao đổi hai ký tự lân cận và mỗi trao đổi như vậy tiêu tốn một đơn vị ngân sách. Mục tiêu của chúng tôi là sắp xếp lại chuỗi thành chuỗi nhị phân nhỏ nhất có thể theo thứ tự từ điển đồng thời tôn trọng giới hạn về số lần hoán đổi mà chúng tôi được phép thực hiện. 

Thứ tự từ điển ở đây là so sánh chuỗi thông thường: chúng tôi muốn càng nhiều chuỗi đứng đầu`0`s nhất có thể và trong số các chuỗi có cùng tiền tố, chúng tôi ưu tiên chuỗi đẩy`1`càng xa về bên phải càng tốt. 

Một cách hữu ích để diễn giải lại hoạt động là nghĩ đến việc di chuyển từng`0`trái qua`1`S. Mỗi lần một`0`vượt qua một`1`, nó tốn đúng một lần hoán đổi. Điều này ngay lập tức gợi ý rằng vấn đề nằm ở việc tham lam tiến tới các số 0 về bên trái trong phạm vi ngân sách cho phép. 

Tổng cộng các ràng buộc rất lớn: tổng độ dài của tất cả các trường hợp thử nghiệm lên tới một triệu và số lần hoán đổi được phép cho mỗi thử nghiệm có thể lớn bằng$n^2$. Điều này loại trừ mọi cách tiếp cận mô phỏng hoán đổi hoặc quét liên tục chuỗi theo phương pháp bậc hai. Thậm chí một$O(nk)$mô phỏng cho mỗi trường hợp thử nghiệm là không thể vì$k$có thể rất lớn. Chúng ta cần một chiến lược tuyến tính hoặc gần tuyến tính cho mỗi bài kiểm tra. 

Một cạm bẫy tinh vi xuất hiện khi nhiều số 0 cạnh tranh nhau để giành những vị trí đầu tiên. Một chiến lược tham lam ngây thơ luôn di chuyển con số 0 hiện tại sang trái hết mức có thể có thể thất bại nếu nó không tính đến ngân sách còn lại trên toàn cầu. Một lỗi phổ biến khác là mô phỏng hoán đổi từng bước, điều này dẫn đến hành vi bậc hai ngay cả đối với đầu vào vừa phải. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực thực hiện các giao dịch hoán đổi một cách tham lam theo đúng nghĩa đen. Người ta có thể quét chuỗi nhiều lần và bất cứ khi nào một`10`đã tìm thấy mẫu, hãy đổi nó nếu chúng ta vẫn còn ngân sách. Điều này tạo ra sự cải thiện từ điển chính xác vì mỗi lần hoán đổi cục bộ sẽ cải thiện thứ tự. Tuy nhiên, trong trường hợp xấu nhất, chẳng hạn như một chuỗi như`111...1000...0`, mỗi số 0 phải vượt qua nhiều số 1 và số lần hoán đổi được thực hiện có thể theo thứ tự$n^2$. Mỗi lần hoán đổi cũng yêu cầu quét hoặc cập nhật các vị trí, dẫn đến độ phức tạp bậc hai hoặc tệ hơn cho mỗi trường hợp kiểm thử, điều này là không khả thi. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng các giao dịch hoán đổi một cách rõ ràng. Thay vào đó, chúng tôi xử lý các số 0 từ trái sang phải và quyết định mỗi số 0 có thể di chuyển sang trái bao xa dựa trên số lượng số 0 hiện đang ở phía trước nó và ngân sách còn lại là bao nhiêu. Mỗi số 0 góp phần tiêu thụ ngân sách bằng số lượng`1`Nó nhảy qua, nhưng nó không thể can thiệp vào các quyết định trước đó ngoài việc giảm ngân sách còn lại. 

Chúng ta có thể duy trì một cấu trúc nơi chúng ta theo dõi xem có bao nhiêu số 0 đã được đặt và bao nhiêu số 0 vẫn có thể vượt qua. Một quan điểm rõ ràng hơn là lặp qua chuỗi trong khi duy trì cấu trúc giống như hàng đợi gồm các số 0 đang chờ xử lý và số lượng giao dịch hoán đổi có sẵn đang chạy, đặt tham lam`0`càng xa càng tốt bất cứ khi nào ngân sách cho phép. 

Một ý tưởng tương đương và thân thiện hơn với việc triển khai là mô phỏng việc xây dựng câu trả lời từ trái sang phải. Ở mỗi vị trí, chúng tôi quyết định liệu chúng tôi có thể mang lại một`0`từ một số vị trí trong tương lai vào vị trí hiện tại. Chúng tôi tham lam chọn những gì có thể tiếp cận sớm nhất`0`có thể được chuyển đến vị trí hiện tại bằng cách sử dụng các giao dịch hoán đổi còn lại. 

Điều này dẫn đến một quá trình trong đó chúng tôi quét chuỗi, duy trì số lượng số 0 vẫn chưa được sử dụng và đối với mỗi vị trí, hãy quyết định xem liệu số 0 có thể được “kéo” vào chuỗi đó theo ràng buộc chi phí còn lại hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trao đổi vũ lực |$O(n^2)$|$O(1)$| Quá chậm | 
| Vị trí ngân sách tham lam |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước, trong khi quét chuỗi, vị trí của tất cả`0`S. Chúng tôi sẽ xem xét việc di chuyển chúng sang trái theo thứ tự xuất hiện. 
2. Duy trì một con trỏ`i`trên câu trả lời cuối cùng đang được xây dựng và một con trỏ khác trên các số 0 trong chuỗi gốc. Cũng duy trì một biến`k`đại diện cho ngân sách hoán đổi còn lại. 
3. Đối với từng vị trí`i`từ trái sang phải, quyết định nhân vật nào sẽ chiếm giữ nó. chúng tôi muốn`0`nếu có thể, vì nó giảm thiểu thứ tự từ điển. 
4. Để quyết định liệu một`0`có thể được đặt ở vị trí`i`, tính chi phí di chuyển phần không sử dụng tiếp theo`0`để định vị`i`. Chi phí này là số lượng ký tự không được di chuyển giữa vị trí ban đầu của nó và`i`, thực tế tương đương với số lần hoán đổi mà nó phải thực hiện để can thiệp chéo`1`S. 
5. Nếu chi phí này nhỏ hơn hoặc bằng phần còn lại`k`, chúng tôi đặt`0`ở vị trí`i`, tiêu thụ chi phí đó và nâng cao con trỏ 0. Ngược lại, chúng ta đặt`1`và tiếp tục. 
6. Tiếp tục cho đến khi tất cả các vị trí được lấp đầy. 

Ý tưởng chính đằng sau bước 4 là mỗi`0`di chuyển độc lập trong cấu trúc từ trái sang phải và chi phí di chuyển của nó chỉ phụ thuộc vào số lượng vị trí mà nó bỏ qua. Bằng cách luôn chọn số 0 sẵn có tiếp theo khi nó phù hợp, chúng tôi đảm bảo rằng các vị trí trước đó nhận được số 0 bất cứ khi nào có thể, đó chính xác là những gì việc giảm thiểu từ điển yêu cầu. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến tham lam: tại mọi vị trí, nếu có thể đặt một`0`ở đó, bằng cách sử dụng một số chuỗi hoán đổi hợp lệ trong ngân sách còn lại, thuật toán sẽ thực hiện như vậy. Bất kỳ giải pháp tối ưu nào cũng phải đặt một`0`trong trường hợp đó, bởi vì thay thế nó bằng`1`sẽ chỉ đẩy một`0`xa hơn nữa là không cần tiết kiệm ngân sách mà có thể cải thiện thứ tự từ điển sớm hơn. Vì các giao dịch hoán đổi chỉ ảnh hưởng đến thứ tự tương đối và mỗi`0`có chi phí cố định để đạt được vị trí sớm hơn, tham lam tiêu thụ những gì rẻ nhất hiện có`0`duy trì sự tối ưu cục bộ và toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n, k = map(int, input().split())
        s = input().strip()

        zeros = []
        ones_before = 0

        for i, ch in enumerate(s):
            if ch == '0':
                zeros.append(i)

        z = 0
        used = [False] * n
        res = []

        for i in range(n):
            # cost to bring next zero here
            if z < len(zeros):
                pos = zeros[z]
                cost = pos - i
                if cost <= k:
                    res.append('0')
                    k -= cost
                    z += 1
                    continue
            res.append('1')

        print("".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai tính toán trước vị trí của tất cả các số 0 và sau đó quyết định xem liệu số 0 chưa sử dụng tiếp theo có thể được chuyển đến vị trí hiện tại hay không. biểu hiện`pos - i`nắm bắt khoảng cách mà số 0 phải dịch chuyển sang trái theo các giao dịch hoán đổi liền kề. Nếu ngân sách cho phép, chúng tôi đặt nó; nếu không chúng tôi đặt`1`. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng rõ ràng các giao dịch hoán đổi. Việc tính toán chi phí ngầm tính đến tất cả các giao dịch hoán đổi liền kề được yêu cầu. Đây là những gì giữ cho giải pháp tuyến tính. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi thuật toán trên hai đầu vào. 

### Ví dụ 1 

đầu vào:```
n=8, k=5
11011010
```Số không ở các vị trí:`[2, 5, 7]`. 

| tôi | vị trí số 0 tiếp theo | chi phí | k trước | hành động | k sau | đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 5 | lấy 0 | 3 | 0 | 
| 1 | 5 | 4 | 3 | không thể lấy | 3 | 01 | 
| 2 | 5 | 3 | 3 | lấy 0 | 0 | 010 | 
| 3 | 7 | 4 | 0 | không thể lấy | 0 | 0101 | 
| 4 | 7 | 3 | 0 | không thể lấy | 0 | 01011 | 
| 5 | 7 | 2 | 0 | không thể lấy | 0 | 010111 | 
| 6 | 7 | 1 | 0 | không thể lấy | 0 | 0101111 | 
| 7 | 7 | 0 | 0 | lấy 0 | 0 | 01011110 | 

Điều này xác nhận rằng các số 0 sẽ được kéo sang trái bất cứ khi nào có thể chấp nhận được và khi ngân sách cạn kiệt, cấu trúc còn lại sẽ được cố định. 

### Ví dụ 2 

đầu vào:```
n=7, k=11
1111100
```Số không ở vị trí`[5, 6]`. 

| tôi | vị trí số 0 tiếp theo | chi phí | k trước | hành động | k sau | đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 5 | 11 | lấy 0 | 6 | 0 | 
| 1 | 6 | 5 | 6 | lấy 0 | 1 | 00 | 
| 2 | (không có) | - | 1 | 1 | 1 | 001 | 
| 3 | (không có) | - | 1 | 1 | 1 | 0011 | 
| 4 | (không có) | - | 1 | 1 | 1 | 00111 | 
| 5 | (không có) | - | 1 | 1 | 1 | 001111 | 
| 6 | (không có) | - | 1 | 1 | 1 | 0011111 | 

Ở đây ngân sách đủ để sắp xếp đầy đủ chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi ký tự được xử lý một lần và mỗi số 0 được nâng lên một lần | 
| Không gian |$O(n)$| Lưu trữ các vị trí 0 và chuỗi đầu ra | 

Tổng độ dài trên tất cả các trường hợp thử nghiệm tối đa là một triệu, do đó, cách tiếp cận thời gian tuyến tính cho mỗi trường hợp thử nghiệm là đủ và chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""3
8 5
11011010
7 9
1111100
7 11
1111100
""") == """01011110
0101111
0011111"""

# minimum size
assert run("""1
1 100
0
""") == "0"

# no budget effect
assert run("""1
5 0
01010
""") == "01010"

# already sorted
assert run("""1
6 3
000111
""") == "000111"

# maximum swaps sufficient
assert run("""1
5 10
11100
""") == "00111"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`0`| xử lý ký tự đơn | 
|`k=0`trường hợp | không thay đổi | ràng buộc không được di chuyển | 
| đã được sắp xếp | cùng một chuỗi | ổn định | 
| cao k | được sắp xếp đầy đủ | chuyển đổi hoàn toàn | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi chuỗi bắt đầu bằng nhiều`1`s và có một số lượng nhỏ`0`nó ở xa bên phải. Thuật toán vẫn hoạt động chính xác vì một số vị trí đầu tiên chỉ có thể được lấp đầy bằng`1`s cho đến khi tích lũy đủ ngân sách để kéo`0`phía trước. 

Một trường hợp cạnh khác là khi`k`là cực kỳ lớn. Trong trường hợp này, mọi số 0 đều trở nên hợp lý ở vị trí sớm nhất có thể của nó và thuật toán suy biến thành một chuỗi được sắp xếp đầy đủ với tất cả`0`s ở phía trước. 

Trường hợp thứ ba là khi các số 0 đậm đặc ở gần cuối. Ngay cả khi có nhiều số 0 tồn tại, mỗi số 0 được coi là chính xác một lần và chi phí của chúng giảm dần khi`i`tăng lên, đảm bảo không có số 0 nào bị bỏ qua không chính xác hoặc bị tính hai lần.
