---
problem: 1294D
contest_id: 1294
problem_index: D
name: "MEX maximizing"
contest_name: "Codeforces Round 615 (Div. 3)"
rating: 1600
tags: ["data structures", "greedy", "implementation", "math"]
answer: passed_samples
verified: true
solve_time_s: 263
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2dc457-23f0-83ec-b993-ecf9e885a0f0
---

#CF 1294D - Tối đa hóa MEX 

**Đánh giá:** 1600 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai, toán học 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 4m 23s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2dc457-23f0-83ec-b993-ecf9e885a0f0 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi đang xây dựng một mảng một phần tử cùng một lúc. Sau mỗi lần chèn, chúng ta được phép “cấu hình lại” các giá trị bằng một thao tác đặc biệt: chúng ta có thể cộng hoặc trừ một số cố định$x$với bất kỳ phần tử nào, bao nhiêu lần tùy thích, miễn là chúng ta không bao giờ xuống dưới 0. Sau khi thực hiện bất kỳ điều chỉnh nào như vậy, chúng tôi muốn biết MEX tối đa có thể có của mảng. 

Điểm mấu chốt là các phần tử mảng không còn là giá trị cố định nữa. Mỗi số$y$chúng tôi chèn không chỉ là một giá trị mà còn là toàn bộ cấp số cộng của các giá trị có thể có:$$y, y + x, y + 2x, \dots$$và cũng di chuyển xuống dưới$x$miễn là chúng ta không âm, nghĩa là các giá trị có thể truy cập được từ$y$chính xác là tất cả các số đều đồng dạng với$y \bmod x$, nhưng bị giới hạn ở số nguyên không âm. 

Vì vậy, sau mỗi truy vấn, chúng tôi muốn biết: tiền tố lớn đến mức nào$[0, 1, 2, \dots]$chúng ta hoàn toàn có thể “bao phủ” bằng cách sử dụng các số có sẵn, trong đó mỗi số đóng góp một vị trí trong modulo lớp dư lượng của nó$x$. 

Các hạn chế lên tới$4 \cdot 10^5$, vì vậy mọi nghiệm đều phải gần tuyến tính hoặc$O(n \log n)$. Bất cứ điều gì cố gắng mô phỏng các hoạt động hoặc duy trì tập hợp đầy đủ các giá trị có thể truy cập cho mỗi phần tử sẽ không thành công vì mỗi truy vấn có thể kích hoạt tính toán lại quy mô lớn trên toàn bộ mảng. 

Một sự hiểu lầm ngây thơ thường phá vỡ các giải pháp là xử lý từng phần tử một cách độc lập và nghĩ rằng chúng ta có thể tự do biến các giá trị thành bất kỳ số nguyên không âm nào. Điều đó sai: chúng tôi bị hạn chế bởi modulo$x$. Ví dụ, nếu$x = 3$, giá trị như 1 chỉ có thể trở thành$1, 4, 7, 10, \dots$, không bao giờ là 2 hoặc 3. 

Một trường hợp thất bại tinh tế xuất hiện khi nhiều phần tử cạnh tranh cho cùng một lớp dư lượng. Ví dụ: nếu nhiều số đồng dạng với 0 mod$x$, tất cả chúng đều “sống” trong cùng một nhóm và không thể được sử dụng độc lập cho tất cả các số nguyên nhỏ được yêu cầu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là duy trì toàn bộ mảng sau mỗi truy vấn và đối với mọi giá trị MEX ứng cử viên$m$, kiểm tra xem nó có thể được hình thành bằng cách sử dụng các phần tử có sẵn theo ràng buộc modulo hay không. Điều này sẽ liên quan đến việc mô phỏng việc gán số cho các mục tiêu$0, 1, 2, \dots$, liên tục kiểm tra tính khả thi cho mỗi truy vấn. Trong trường hợp xấu nhất, điều này trở thành$O(n^2)$hoặc tệ hơn, vì mỗi truy vấn có thể yêu cầu quét nhiều giá trị và có khả năng điều chỉnh nhiều phần tử. 

Cái nhìn sâu sắc về cấu trúc quan trọng là mỗi số chỉ quan trọng thông qua modulo còn lại của nó.$x$, bởi vì hoạt động được phép sẽ bảo toàn lớp dư lượng đó. Vậy mọi phần tử đều thuộc về đúng một trong$x$xô độc lập. 

Bây giờ hãy xem xét việc xây dựng MEX từ$0$trở lên. Để tạo thành một số$m$, chúng ta phải có ít nhất một phần tử chưa được sử dụng trong nhóm$m \bmod x$. Mỗi phần tử chỉ có thể được sử dụng một lần, do đó, mỗi nhóm hoạt động giống như một hàng các “khe” có sẵn cho các số có phần còn lại đó. Khi chúng ta gán số$0, 1, 2, \dots$, chúng tôi tiêu thụ một phần tử từ mỗi nhóm dư lượng tương ứng. 

Vì vậy, vấn đề giảm xuống còn việc duy trì số lần mỗi lớp còn lại được nhìn thấy và tham lam kiểm tra MEX bị thiếu nhỏ nhất. 

Thách thức là duy trì điều này một cách linh hoạt khi các truy vấn đến. Chúng tôi duy trì một con trỏ cho mỗi lớp dư lượng cho biết có bao nhiêu phần tử trong lớp đó đã được sử dụng để hình thành các giá trị MEX trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(x)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc chính: bộ đếm tần số cho mỗi modulo lớp dư lượng$x$và một con trỏ toàn cục theo dõi ứng cử viên MEX nhỏ nhất. 

Mỗi lớp dư lượng$r$lưu trữ bao nhiêu phần tử với$y \equiv r \pmod{x}$đã được chèn vào. 

1. Đối với mỗi giá trị đến$y$, tính toán$r = y \bmod x$và tăng số lượng nhóm$r$. Điều này phản ánh việc thêm một phần tử có thể sử dụng được nữa vào lớp dư lượng đó. 
2. Duy trì con trỏ MEX hiện tại$mex$, bắt đầu từ 0. Giá trị này thể hiện giá trị nhỏ nhất mà chúng tôi chưa xác nhận là có thể xây dựng được. 
3. Sau mỗi lần chèn, liên tục kiểm tra xem dòng điện có$mex$có thể được hình thành: 

cái này đòi hỏi cái xô đó$mex \bmod x$có nhiều phần tử hơn số lần chúng tôi đã sử dụng lớp dư lượng đó cho các giá trị MEX trước đó. 
4. Nếu có thể, chúng ta “tiêu thụ” một phần tử từ lớp dư lượng đó và tăng dần$mex$. Mô hình này gán một phần tử chưa được sử dụng trước đó để biểu thị giá trị MEX này. 
5. Dừng lại ở lần tiếp theo$mex$không thể được hình thành do không đủ thành phần trong thùng cặn của nó. Đầu ra hiện tại$mex$. 

Lý do chúng ta chỉ di chuyển$mex$chuyển tiếp là khi một số được xác nhận là không thể tại một thời điểm nhất định, nó có thể trở thành khả thi sau này khi có nhiều phần tử hơn xuất hiện, nhưng các giá trị trước đó không bao giờ được xem lại. 

### Tại sao nó hoạt động 

Tính bất biến là ở mọi thời điểm, với mọi giá trị$v < mex$, chúng tôi đã khớp nó với một phần tử riêng biệt có lớp dư lượng cho phép điều đó. Đối với mỗi như vậy$v$, chúng tôi đã sử dụng chính xác một phần tử từ nhóm$v \bmod x$. 

Khi chúng tôi cố gắng mở rộng đến$mex$, chúng tôi đang kiểm tra xem có tồn tại phần tử chưa được sử dụng trong lớp dư lượng được yêu cầu hay không. Nếu có, chúng tôi sẽ giao nó vĩnh viễn cho$mex$. Nếu không, việc gán lại các phần tử trước đó trong tương lai sẽ không thể giúp ích mà không vi phạm nhiệm vụ trước đó, vì những phần tử đó đã được cam kết. Điều này làm cho phép gán tham lam trở nên tối ưu và đảm bảo rằng$mex$luôn được tối đa hóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q, x = map(int, input().split())
    cnt = [0] * x
    
    mex = 0
    
    for _ in range(q):
        y = int(input())
        r = y % x
        cnt[r] += 1
        
        # try to extend mex greedily
        while cnt[mex % x] > mex // x:
            mex += 1
        
        print(mex)

if __name__ == "__main__":
    solve()
```Giải pháp chỉ duy trì số lượng dư lượng và một con trỏ tăng dần. điều kiện`cnt[mex % x] > mex // x`nắm bắt chính xác liệu chúng tôi có đủ phần tử trong lớp dư lượng đó để gán cho vị trí MEX hiện tại hay không. 

Sự phân chia`mex // x`biểu thị số lượng trước đó trong lớp dư lượng này đã được sử dụng trong khi xây dựng các phần trước đó của chuỗi MEX. 

Vòng lặp chỉ tiến về phía trước nên toàn bộ chuyển động của`mex`trên tất cả các truy vấn là tuyến tính. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

đầu vào:```
7 3
0
1
2
2
0
0
10
```Chúng tôi theo dõi`cnt`Và`mex`: 

| Bước | y | cnt mod 3 (0,1,2) | mex | 
| --- | --- | --- | --- | 
| 1 | 0 | (1,0,0) | 1 | 
| 2 | 1 | (1,1,0) | 2 | 
| 3 | 2 | (1,1,1) | 3 | 
| 4 | 2 | (1,1,2) | 3 | 
| 5 | 0 | (2,1,2) | 4 | 
| 6 | 0 | (3,1,2) | 4 | 
| 7 | 10 | (4,1,2) | 7 | 

Điều này xác nhận rằng ngay khi mỗi lớp dư lượng tích lũy đủ công suất, MEX sẽ nhảy về phía trước theo từng khối. 

Dấu vết cho thấy phần dư 0 trở nên phổ biến như thế nào và cuối cùng cho phép bỏ qua nhiều giá trị bị thiếu theo trình tự. 

### Ví dụ căng thẳng nhỏ 

đầu vào:```
5 2
1
3
5
7
9
```Tất cả các số đều là số lẻ nên chỉ có dư lượng loại 1 tăng lên. 

| Bước | cnt (0,1) | mex | 
| --- | --- | --- | 
| 1 | (0,1) | 0 | 
| 2 | (0,2) | 0 | 
| 3 | (0,3) | 0 | 
| 4 | (0,4) | 0 | 
| 5 | (0,5) | 0 | 

Chúng ta không bao giờ có thể tạo thành 0 vì số dư 0 không bao giờ xuất hiện, vì vậy MEX luôn bằng 0. 

Điều này chứng tỏ rằng sự sẵn có của tất cả các loại dư lượng là cần thiết cho sự tiến bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q)$| Mỗi phần tử tăng một bộ đếm và con trỏ MEX chỉ di chuyển về phía trước trên tất cả các giá trị nhiều nhất$q$lần | 
| Không gian |$O(x)$| Chỉ số lượng lớp dư lượng được lưu trữ | 

Thuật toán phù hợp thoải mái trong giới hạn vì cả hai$q$Và$x$đang lên đến$4 \cdot 10^5$và việc sử dụng bộ nhớ là tuyến tính trong$x$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    _stdout = _sys.stdout
    out = io.StringIO()
    _sys.stdout = out
    solve()
    _sys.stdout = _stdout
    return out.getvalue().strip()

# provided sample
assert run("""7 3
0
1
2
2
0
0
10
""") == """1
2
3
3
4
4
7"""

# minimum case
assert run("""1 1
0
""") == "1"

# all same residue, cannot progress
assert run("""3 5
1
6
11
""") == "0\n0\n0"

# full coverage small
assert run("""4 2
0
1
2
3
""") == "1\n2\n3\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| chỉ dư lượng duy nhất | tất cả 0 | không thể vì thiếu lớp | 
| dư lượng xen kẽ | tăng MEX | tiến triển bình thường | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chỉ có một lớp dư lượng xuất hiện trong tất cả các truy vấn. Trong tình huống đó, chỉ những số phù hợp với lớp đó theo modulo$x$có thể được hình thành nên MEX dừng ở mức 0 ngay lập tức. Thuật toán xử lý việc này vì`cnt[mex % x]`sẽ không bao giờ vượt quá 0 khi`mex % x`là lớp còn thiếu. 

Một trường hợp khác là khi$x = 1$. Sau đó, mọi số đều thuộc cùng một lớp dư lượng và mỗi lần chèn trực tiếp sẽ làm tăng nhóm có sẵn. Điều kiện được đơn giản hóa thành so sánh số lượng thuần túy và MEX trở thành bằng số phần tử được xử lý mà thuật toán tạo ra một cách tự nhiên vì`cnt[0]`luôn phát triển và`mex`tiến lên từng bước.
