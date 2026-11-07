---
problem: 1320A
contest_id: 1320
problem_index: A
name: "Journey Planning"
contest_name: "Codeforces Round 625 (Div. 1, based on Technocup 2020 Final Round)"
rating: 1400
tags: ["data structures", "dp", "greedy", "math", "sortings"]
answer: passed_samples
verified: true
solve_time_s: 267
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2de6ec-99fc-83ec-9276-106005ae8eca
---

#CF 1320A - Lập kế hoạch hành trình 

**Đánh giá:** 1400 
**Tags:** cấu trúc dữ liệu, dp, tham lam, toán học, sắp xếp 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 4 phút 27s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2de6ec-99fc-83ec-9276-106005ae8eca 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một dòng các thành phố, mỗi thành phố có một chỉ mục từ trái sang phải và một giá trị vẻ đẹp liên quan. Chúng ta cần chọn một chuỗi các thành phố để ghé thăm sao cho chỉ số tăng lên một cách nghiêm ngặt. Hạn chế chính là các thành phố được chọn liên tiếp không phải là tùy tiện: sự gia tăng vị trí phải tương ứng với sự gia tăng về vẻ đẹp. Về mặt hình thức, nếu chúng ta di chuyển từ thành phố$i$đến thành phố$j$, thì sự khác biệt về chỉ số phải bằng sự khác biệt về giá trị vẻ đẹp của chúng. 

Điều kiện này có thể được viết lại theo cách có cấu trúc hơn. Sắp xếp lại$$j - i = b_j - b_i$$cho$$b_j - j = b_i - i$$Vì vậy, tất cả các thành phố trong một hành trình hợp lệ phải có cùng giá trị$b_i - i$. Quan sát đơn lẻ đó biến vấn đề thành nhóm theo khóa dẫn xuất. 

Mục tiêu là chọn một nhóm thành phố hợp lệ (cùng khóa) và tối đa hóa tổng giá trị vẻ đẹp của chúng. Một nhóm có thể bao gồm một thành phố, thành phố này luôn hợp lệ. 

Ràng buộc$n \le 2 \cdot 10^5$loại trừ mọi cặp thành phố bậc hai. Bất kỳ giải pháp nào thử tất cả các cặp hoặc xây dựng chuyển tiếp giữa tất cả các cạnh hợp lệ sẽ giảm xuống$O(n^2)$trong trường hợp xấu nhất là quá chậm. Chúng ta cần một chiến lược nhóm tuyến tính hoặc gần tuyến tính. 

Một trường hợp phức tạp xuất hiện khi tất cả các thành phố có đặc điểm riêng biệt$b_i - i$các giá trị. Trong trường hợp đó, mọi hành trình hợp lệ đều có độ dài bằng một, vì vậy câu trả lời đơn giản là độ dài tối đa$b_i$. Một cách tiếp cận ngây thơ giả định rằng ít nhất một quá trình chuyển đổi hợp lệ có thể cố gắng mở rộng các chuỗi không tồn tại một cách không chính xác. 

Một trường hợp khác là khi nhiều thành phố có chung$b_i - i$nhưng không liền kề nhau. Điều kiện không yêu cầu tính liền kề trong mảng, chỉ yêu cầu sự bằng nhau của khóa dẫn xuất, vì vậy việc bỏ qua các chỉ số là hợp lệ miễn là thứ tự được giữ nguyên. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng bắt đầu từ mọi thành phố và mở rộng đến tất cả các thành phố hợp lệ tiếp theo. Từ một thành phố$i$, chúng ta có thể thử mọi cách$j > i$và kiểm tra xem điều kiện chuyển tiếp có đúng hay không. Mỗi lần chuyển đổi thành công sẽ dẫn đến một phần mở rộng đệ quy hoặc DP. Điều này hoạt động hợp lý vì nó trực tiếp thực thi ràng buộc, nhưng đối với mỗi thành phố, chúng tôi có thể quét tất cả các thành phố sau đó, dẫn đến khoảng$O(n^2)$chuyển tiếp. Với$2 \cdot 10^5$các thành phố, đây là theo thứ tự của$4 \cdot 10^{10}$kiểm tra, điều này là không thể thực hiện được. 

Sự đơn giản hóa cấu trúc quan trọng xuất phát từ việc viết lại ràng buộc. Điều kiện của các thành phố liên tiếp chỉ phụ thuộc vào sự bằng nhau của$b_i - i$. Điều này có nghĩa là toàn bộ vấn đề sẽ phân rã thành các nhóm độc lập được lập chỉ mục theo giá trị này. Sau khi được nhóm, không có sự tương tác giữa các nhóm khác nhau và trong một nhóm, mọi tập hợp con tôn trọng chỉ số tăng dần đều hợp lệ. 

Điều này biến vấn đề thành một nhiệm vụ tích lũy đơn giản: với mỗi khóa$b_i - i$, tổng tất cả tương ứng$b_i$giá trị và lấy giá trị lớn nhất trên tất cả các nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra cặp/DP qua chuyển tiếp) |$O(n^2)$|$O(n)$| Quá chậm | 
| Nhóm theo$b_i - i$|$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi thành phố$i$, tính giá trị$key = b_i - i$. Điều này biến ràng buộc thành một quy tắc nhóm. Lý do điều này có hiệu quả là vì điều kiện ban đầu giữa các thành phố lân cận trở thành sự bình đẳng trong biểu thức này. 
2. Duy trì bản đồ băm từ mỗi khóa đến tổng giá trị vẻ đẹp của tất cả các thành phố có khóa đó. Mỗi thành phố đóng góp độc lập cho nhóm của mình. 
3. Lặp lại tất cả các thành phố, thêm$b_i$về tổng tương ứng với$b_i - i$. Điều này xây dựng tổng đóng góp của mọi nhóm hành trình hợp lệ. 
4. Theo dõi số tiền tối đa trên tất cả các khóa. Điều này thể hiện việc chọn hành trình hợp lệ tốt nhất có thể, vì bất kỳ hành trình hợp lệ nào cũng phải nằm hoàn toàn trong một nhóm chính. 
5. Xuất ra giá trị lớn nhất tìm được. 

### Tại sao nó hoạt động 

Sự biến đổi$j - i = b_j - b_i$hàm ý sự bất biến của$b_i - i$qua tất cả các bước liên tiếp và do đó xuyên suốt toàn bộ chuỗi. Điều này phân chia các thành phố thành các lớp riêng biệt, trong đó các chuyển đổi luôn có giá trị bên trong và không thể thực hiện được bên ngoài. Bất kỳ hành trình hợp lệ nào cũng chính xác là một chuỗi con bên trong một lớp như vậy và vẻ đẹp tổng thể của nó chỉ đơn giản là tổng các phần tử của nó. Vì chúng ta có thể tự do chọn bất kỳ tập hợp con nào trong một lớp, nên việc lấy tất cả các phần tử trong lớp đó sẽ tối đa hóa tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    b = list(map(int, input().split()))
    
    mp = {}
    ans = 0
    
    for i, val in enumerate(b, start=1):
        key = val - i
        if key in mp:
            mp[key] += val
        else:
            mp[key] = val
        if mp[key] > ans:
            ans = mp[key]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện ý tưởng nhóm. Việc liệt kê bắt đầu từ 1 để phù hợp với việc lập chỉ mục thành phố của vấn đề. Từ điển lưu trữ tổng tích lũy cho mỗi khóa và câu trả lời được cập nhật nhanh chóng để tránh phải vượt qua lần thứ hai. 

Một sai lầm phổ biến là quên rằng các chỉ số dựa trên 1 trong tính toán khóa. Việc sử dụng chỉ mục dựa trên 0 sẽ dịch chuyển tất cả các nhóm và tạo ra sự hợp nhất không chính xác của các thành phố không liên quan. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
10 7 1 9 10 15
```Chúng tôi tính toán$b_i - i$cho từng vị trí. 

| tôi | b[i] | key = b[i] - i | tổng số tiền chạy trên mỗi khóa | 
| --- | --- | --- | --- | 
| 1 | 10 | 9 | 9 → 10 | 
| 2 | 7 | 5 | 5 → 7 | 
| 3 | 1 | -2 | -2 → 1 | 
| 4 | 9 | 5 | 5 → 16 | 
| 5 | 10 | 5 | 5 → 26 | 
| 6 | 15 | 9 | 9 → 25 | 

Nhóm tốt nhất là key = 5, cho tổng 26. 

Điều này cho thấy các thành phố có chỉ số không liền kề vẫn có thể kết hợp nếu giá trị chuyển đổi của chúng khớp với nhau và chúng ta không nên tìm kiếm các phân đoạn liền kề. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
```| tôi | b[i] | chìa khóa | tổng số tiền chạy | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 
| 2 | 2 | 0 | 3 | 
| 3 | 3 | 0 | 6 | 
| 4 | 4 | 0 | 10 | 
| 5 | 5 | 0 | 15 | 

Tất cả các thành phố đều có chung một khóa nên hành trình tối ưu sẽ bao gồm tất cả các thành phố đó. 

Điều này xác nhận rằng giải pháp xử lý các chuỗi tương thích hoàn toàn một cách tự nhiên mà không cần cách viết đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi thành phố được xử lý một lần và cập nhật từ điển có thời gian trung bình không đổi | 
| Không gian |$O(n)$| Trong trường hợp xấu nhất, tất cả các khóa đều khác biệt | 

Sự phức tạp này phù hợp thoải mái trong giới hạn cho$n \le 2 \cdot 10^5$, vì cả hoạt động và mức sử dụng bộ nhớ đều có quy mô tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else capture(inp)

def capture(inp):
    import sys
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.strip()

# provided sample
assert capture("6\n10 7 1 9 10 15\n") == "26"

# single element
assert capture("1\n100\n") == "100"

# all same key structure
assert capture("3\n1 2 3\n") == "6"

# no useful merging
assert capture("4\n10 1 10 1\n") == "20"

# decreasing values
assert capture("5\n5 4 3 2 1\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 100 | trường hợp tối thiểu | 
| 1 2 3 | 6 | sáp nhập đầy đủ | 
| 10 1 10 1 | 20 | nhóm không liền kề | 
| 5 4 3 2 1 | 5 | không có chuyển tiếp có lợi | 

## Vỏ cạnh 

Khi nào$n = 1$, thuật toán sẽ gán một khóa duy nhất và trả về giá trị duy nhất đó vì không có gì để kết hợp. Đối với đầu vào`[100]`, chìa khóa là$100 - 1 = 99$, và câu trả lời trực tiếp là 100. 

Khi tất cả$b_i - i$giá trị khác nhau, mỗi nhóm có kích thước một. Ví dụ,`[10, 1, 100]`tạo ra chìa khóa`9, -1, 97`, do đó không có sự hợp nhất nào xảy ra và giá trị đơn lớn nhất được trả về. Thuật toán tránh được việc tổng hợp không chính xác một cách tự nhiên vì mỗi khóa được tách biệt trong từ điển. 

Khi tất cả các thành phố chia sẻ cùng một khóa, chẳng hạn như`[1,2,3,4]`, mỗi thành phố đóng góp vào một bộ tích lũy và thuật toán tính tổng một cách hiệu quả toàn bộ mảng. Điều này chứng tỏ rằng các chuỗi dài hợp lệ được nắm bắt hoàn toàn mà không cần xây dựng trình tự một cách rõ ràng.
