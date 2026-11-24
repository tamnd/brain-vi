---
problem: 959B
contest_id: 959
problem_index: B
name: "Mahmoud and Ehab and the message"
contest_name: "Codeforces Round 473 (Div. 2)"
rating: 1200
tags: ["dsu", "greedy", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 67
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a327fbf-b66c-83ec-981d-3b18ee9546df
---

# CF 959B - Mahmoud và Ehab và thông điệp 

**Đánh giá:** 1200 
**Tags:** dsu, tham lam, thực hiện 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a327fbf-b66c-83ec-981d-3b18ee9546df 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một từ vựng trong đó mỗi từ có giá gửi cố định và một số từ được nhóm lại với nhau vì chúng có cùng ý nghĩa. Trong một nhóm, bất kỳ từ nào cũng có thể được thay thế bằng bất kỳ từ nào khác trong cùng nhóm đó mà không làm thay đổi ý nghĩa của thông báo. 

Một tin nhắn là một chuỗi các từ. Đối với mỗi từ trong tin nhắn, chúng ta được phép gửi nguyên từ đó hoặc thay thế bằng một từ khác trong nhóm tương đương của nó. Chi phí chúng ta phải trả cho vị trí đó chỉ đơn giản là chi phí của từ ngữ chúng ta chọn gửi đi. 

Mục tiêu là giảm thiểu tổng chi phí gửi toàn bộ tin nhắn. 

Cấu trúc quan trọng là mỗi từ thuộc chính xác một nhóm và chỉ được phép thay thế trong nhóm đó. Điều này biến vấn đề thành việc lựa chọn, đối với mỗi từ trong thông điệp, đại diện rẻ nhất trong nhóm của nó. 

Các giới hạn lên tới 100.000 từ, nhóm và độ dài tin nhắn. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng khám phá các từ thay thế trên mỗi từ bằng cách sử dụng các tìm kiếm lồng nhau trên các thành viên nhóm. Việc quét từng từ đơn giản trong nhóm của nó đã quá chậm nếu các nhóm lớn, vì tổng công việc có thể giảm xuống theo hành vi bậc hai. 

Một trường hợp thất bại tinh tế xuất hiện khi một nhóm chứa một từ rất đắt tiền và một từ rất rẻ tiền. Nếu chúng ta lầm tưởng rằng chúng ta nên luôn giữ nguyên từ gốc thì chúng ta sẽ bỏ lỡ những cải tiến. Ví dụ, nếu một nhóm được`{A(cost 100), B(cost 1)}`và tin nhắn chứa`A`, câu trả lời tối ưu sử dụng`B`thay vì. Bất kỳ giải pháp nào không tính toán trước chi phí tối thiểu cho mỗi nhóm sẽ thất bại ở đây. 

Một trường hợp khác phát sinh khi các nhóm là đơn lẻ. Trong trường hợp đó, không thể thay thế được nên phải sử dụng trực tiếp giá gốc. Điều này không phá vỡ logic nhưng rất dễ bị xử lý sai nếu quá trình xử lý nhóm giả định ít nhất hai phần tử. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xử lý từng từ thông báo một cách độc lập. Đối với mỗi từ trong tin nhắn, chúng tôi xác định nhóm của nó và quét tất cả các từ trong nhóm đó để tìm chi phí tối thiểu. Nếu chúng ta giả sử việc phân nhóm trong trường hợp xấu nhất trong đó hầu hết tất cả các từ đều thuộc một nhóm lớn thì mỗi truy vấn có thể có chi phí O(n) và với m truy vấn, điều này dẫn đến O(nm), quá lớn đối với các ràng buộc 10^5. 

Điều quan trọng cần lưu ý là chi phí của thông điệp chỉ phụ thuộc vào chi phí tối thiểu trong mỗi nhóm chứ không phụ thuộc vào cấu trúc thay thế. Khi chúng ta biết từ rẻ nhất trong mỗi lớp tương đương, mỗi lần xuất hiện của bất kỳ từ nào trong lớp đó đều có thể được thay thế bằng từ rẻ nhất đó. 

Điều này làm giảm vấn đề xuống còn một bước tiền xử lý: tính toán, cho mỗi nhóm, chi phí tối thiểu giữa các thành viên của nhóm đó. Sau đó ánh xạ từng từ với chi phí tối thiểu của nhóm nó và tính tổng qua tin nhắn. 

Chúng ta có thể triển khai điều này một cách hiệu quả bằng cách sử dụng bản đồ băm từ từ này sang chỉ mục khác, chỉ mục ánh xạ mảng tới id nhóm và chi phí tối thiểu của nhóm lưu trữ mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O(n + m + n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các từ và gán cho mỗi từ một chỉ mục. 

Điều này cho phép tra cứu liên tục theo thời gian từ các chuỗi thông báo đến chi phí và nhóm của chúng. 
2. Đọc mảng chi phí và liên kết từng chi phí với chỉ số từ của nó. 

Bây giờ mỗi từ đều biết chi phí gửi của nó. 
3. Xây dựng ánh xạ từ mỗi chỉ mục từ tới id nhóm của nó. 

Vì mỗi từ thuộc về chính xác một nhóm nên đây là bài tập trực tiếp. 
4. Đối với mỗi nhóm, hãy tính chi phí tối thiểu trong số tất cả các từ trong nhóm đó. 

Chúng tôi quét các thành viên nhóm một lần và duy trì mức hoạt động tối thiểu. 
5. Đối với mỗi từ trong tin nhắn, hãy chuyển đổi nó thành chỉ mục của nó, sau đó thay thế giá của nó bằng giá tối thiểu của nhóm được tính toán trước. 

Đây là mô hình thay thế tốt nhất có thể được phép. 
6. Tính tổng các chi phí tối thiểu này cho tất cả các từ trong thông báo và đưa ra kết quả. 

### Tại sao nó hoạt động 

Mỗi từ thông báo là độc lập về các lựa chọn thay thế. Hạn chế duy nhất là những người thay thế phải ở trong cùng một nhóm. Vì không có sự tương tác giữa các vị trí trong tin nhắn nên việc tối ưu hóa từng vị trí một cách độc lập là tối ưu toàn cầu. Trong một nhóm, việc chọn bất cứ thứ gì ngoài từ có chi phí tối thiểu chỉ có thể làm tăng chi phí, vì vậy mức tối thiểu luôn là tối ưu cho mọi lần xuất hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, m = map(int, input().split())
    
    words = input().split()
    cost = list(map(int, input().split()))
    
    idx = {w: i for i, w in enumerate(words)}
    
    group = [-1] * n
    group_min = [10**18] * k
    
    for gid in range(k):
        data = list(map(int, input().split()))
        x = data[0]
        members = data[1:]
        for v in members:
            v -= 1
            group[v] = gid
            group_min[gid] = min(group_min[gid], cost[v])
    
    msg = input().split()
    
    ans = 0
    for w in msg:
        i = idx[w]
        g = group[i]
        ans += group_min[g]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng một từ điển từ các từ đến chỉ mục để việc tra cứu tin nhắn diễn ra liên tục. Sau đó, nó gán từng từ cho một nhóm đồng thời tính toán chi phí tối thiểu cho nhóm đó. Vòng lặp cuối cùng qua tin nhắn chỉ đơn giản dịch từng từ thành id nhóm của nó và tích lũy chi phí tối thiểu tương ứng. Việc trừ 1 từ các chỉ số là cần thiết vì các nhóm đầu vào dựa trên 1. 

Một sai lầm phổ biến là tính toán lại giá trị cực tiểu của nhóm trong thời gian truy vấn thay vì xử lý trước. Điều đó sẽ biến mỗi lần tra cứu thư thành một lần quét toàn bộ nhóm, việc này không cần thiết và quá chậm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4 4
i loser am the second
100 1 1 5 10
1 1
1 3
2 2 5
1 4
i am the second
```| từ | chỉ mục | nhóm | chi phí tối thiểu của nhóm | chi phí sử dụng | 
| --- | --- | --- | --- | --- | 
| tôi | 0 | 0 | 100 | 100 | 
| sáng | 2 | 1 | 1 | 1 | 
| | 3 | 3 | 5 | 5 | 
| thứ hai | 4 | 2 | 1 | 1 | 

Tổng số là 107. 

Dấu vết này cho thấy dù “thứ hai” đắt tiền nhưng nó lại được thay thế bằng từ rẻ nhất trong nhóm của nó, đó là “kẻ thua cuộc”. 

### Ví dụ 2 

đầu vào:```
3 2 3
a b c
10 20 30
1 1
2 2 3
a b c
```| từ | chỉ mục | nhóm | chi phí tối thiểu của nhóm | chi phí sử dụng | 
| --- | --- | --- | --- | --- | 
| một | 0 | 0 | 10 | 10 | 
| b | 1 | 1 | 20 | 20 | 
| c | 2 | 1 | 20 | 20 | 

Tổng cộng là 50. 

Điều này xác nhận rằng trong một nhóm chứa nhiều từ, tất cả các lần xuất hiện đều sử dụng cùng một đại diện rẻ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k + m) | Mỗi từ, thành viên nhóm và mã thông báo tin nhắn được xử lý một lần | 
| Không gian | O(n) | Mảng để ánh xạ từ, gán nhóm và cực tiểu nhóm | 

Giải pháp này dễ dàng phù hợp trong giới hạn vì tất cả các thao tác đều tuyến tính ở kích thước đầu vào và việc tra cứu từ điển có thời gian trung bình không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    return main(inp) if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k, m = map(int, input().split())
    words = input().split()
    cost = list(map(int, input().split()))
    idx = {w: i for i, w in enumerate(words)}

    group = [-1] * n
    group_min = [10**18] * k

    for gid in range(k):
        data = list(map(int, input().split()))
        x = data[0]
        members = data[1:]
        for v in members:
            v -= 1
            group[v] = gid
            group_min[gid] = min(group_min[gid], cost[v])

    msg = input().split()

    ans = 0
    for w in msg:
        ans += group_min[group[idx[w]]]

    return str(ans)

# provided sample
assert solve_capture("""5 4 4
i loser am the second
100 1 1 5 10
1 1
1 3
2 2 5
1 4
i am the second
""") == "107"

# singleton groups
assert solve_capture("""3 3 3
a b c
5 1 10
1 1
1 2
1 3
a b c
""") == "16"

# all in one group
assert solve_capture("""3 1 3
a b c
10 5 7
3 1 2 3
a b c
""") == "15"

# already optimal
assert solve_capture("""2 2 2
x y
3 4
1 1
1 2
x y
""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 107 | tính đúng đắn cơ bản | 
| nhóm đơn | 16 | không thể thay thế được | 
| tất cả trong một nhóm | 15 | tuyên truyền tối thiểu toàn cầu | 
| đã tối ưu | 7 | không có sự thay thế không cần thiết | 

## Vỏ cạnh 

Một nhóm đơn lẻ như`1 1`đảm bảo rằng từ này không có lựa chọn thay thế. Thuật toán chỉ định chi phí của chính nó là mức tối thiểu của nhóm, do đó các truy vấn sử dụng chính xác giá trị ban đầu. 

Một nhóm lớn chứa tất cả các từ sẽ kiểm tra xem quá trình tiền xử lý có truyền chính xác mức tối thiểu duy nhất trên tất cả các vị trí thông báo hay không. Thuật toán tính toán mức tối thiểu toàn cầu và áp dụng nó một cách nhất quán. 

Thông báo lặp lại cùng một từ nhiều lần sẽ kiểm tra xem việc tra cứu có ổn định không và không tính toán lại hoặc thay đổi trạng thái. Mỗi lần xuất hiện sẽ tìm nạp độc lập mức tối thiểu của nhóm được tính toán trước, duy trì tính chính xác.
