---
title: "CF 1850D - Vòng cân bằng"
description: "Chúng ta được cung cấp một danh sách các vấn đề khó khăn và chúng ta được phép loại bỏ bất kỳ tập hợp con nào trong số đó và sau đó sắp xếp lại các vấn đề còn lại một cách tùy ý. Sau khi sắp xếp lại, chúng ta muốn chuỗi trở nên “trơn tru” theo nghĩa là mỗi cặp liền kề khác nhau tối đa $k$."
date: "2026-06-09T05:32:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1850
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 886 (Div. 4)"
rating: 900
weight: 1850
solve_time_s: 190
verified: false
draft: false
---

[CF 1850D - Vòng cân bằng](https://codeforces.com/problemset/problem/1850/D) 

**Xếp hạng:** 900 
**Tags:** vũ phu, tham lam, thực hiện, phân loại 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các vấn đề khó khăn và chúng ta được phép loại bỏ bất kỳ tập hợp con nào trong số đó và sau đó sắp xếp lại các vấn đề còn lại một cách tùy ý. Sau khi sắp xếp lại, chúng ta muốn dãy được “trơn tru” theo nghĩa là mỗi cặp liền kề khác nhau nhiều nhất là$k$. 

Mục tiêu không phải là tự xây dựng trình tự mà là giữ lại càng nhiều vấn đề càng tốt để tồn tại một trật tự như vậy. Tương tự, chúng ta muốn tìm số phần tử tối đa mà chúng ta có thể giữ lại để sắp xếp thành một chuỗi hợp lệ, sau đó trừ đi số đó từ$n$. 

Quan sát quan trọng ẩn giấu trong quyền tự do sắp xếp lại là các ràng buộc kề chỉ phụ thuộc vào việc liệu chúng ta có thể kết nối các giá trị thông qua các bước có kích thước nhiều nhất hay không.$k$, không ở vị trí ban đầu của chúng. 

Từ những hạn chế,$n$có thể lên đến$2 \cdot 10^5$qua các trường hợp thử nghiệm, do đó, mọi phép tính bậc hai trong một trường hợp thử nghiệm đều đã quá chậm. Các giải pháp quét tuyến tính hoặc dựa trên sắp xếp được mong đợi. Vì giá trị tăng lên$10^9$, chúng ta không thể dựa vào mảng tần số hoặc lập chỉ mục trực tiếp. 

Một trường hợp lỗi nhỏ xuất hiện khi các giá trị “gần như được kết nối” nhưng yêu cầu bỏ qua các phần tử trung gian. 

Ví dụ, nếu$k = 2$và các giá trị là$[1, 10, 11, 12]$, một ý tưởng ngây thơ có thể cố gắng mở rộng một cách tham lam từ 1, nhưng 1 không thể kết nối với 10, mặc dù 10, 11, 12 tạo thành một khối hợp lệ dài. Câu trả lời đúng phụ thuộc vào việc xác định cấu trúc lớn nhất trong đó các giá trị liên tiếp nằm trong khoảng cách$k$, không phải bắt đầu từ một điểm tùy ý. 

Một cạm bẫy khác là giả định rằng chúng ta cần duy trì trật tự toàn cầu hoặc mô phỏng các quá trình chuyển đổi. Vì việc sắp xếp lại là miễn phí nên vấn đề giảm xuống còn việc chọn nhiều tập hợp có thể được sắp xếp thành một chuỗi có các khoảng trống giới hạn. 

## Phương pháp tiếp cận 

Cách nhìn bạo lực là thử từng tập con của vấn đề, và với mỗi tập con cố gắng sắp xếp chúng theo thứ tự nào đó và kiểm tra xem tất cả các khác biệt liền kề có nằm trong phạm vi không.$k$. Ngay cả khi chúng tôi giả sử việc kiểm tra một tập hợp con cố định là$O(n \log n)$sau khi sắp xếp thì số tập con là$2^n$, điều này là không thể ngay cả đối với$n = 40$. Điều này cho thấy rõ rằng chúng ta cần rút gọn cấu trúc hơn là liệt kê. 

Thông tin chi tiết quan trọng đến từ việc sắp xếp mảng. Sau khi được sắp xếp, mọi sự sắp xếp hợp lệ đều phải xuất phát từ việc chọn các phần tử có thể được xâu chuỗi theo thứ tự được sắp xếp. Nếu hai phần tử được chọn khác nhau nhiều hơn$k$, chúng không thể liền kề theo bất kỳ thứ tự hợp lệ nào mà không phá vỡ tính đơn điệu ở một nơi khác, bởi vì việc chèn các giá trị trung gian sẽ chỉ làm trầm trọng thêm các khoảng trống. 

Vì vậy, bài toán trở thành tìm tập con lớn nhất sao cho sau khi sắp xếp, các phần tử được chọn liên tiếp khác nhau nhiều nhất là$k$. Điều này tương đương với việc tìm đoạn dài nhất mà chúng ta có thể đi qua trong khi vẫn duy trì các khoảng trống ≤$k$, nhưng có một điểm khác biệt: chúng tôi được phép bỏ qua các phần tử, vì vậy chúng tôi thực sự đang tìm kiếm thành phần được kết nối lớn nhất theo mối quan hệ “sự khác biệt ≤ k giữa một số lân cận trong chuỗi”. 

Theo thứ tự được sắp xếp, chúng ta có thể nghĩ đến việc xây dựng một biểu đồ trong đó các cạnh kết nối các chỉ số$i, j$nếu như$a_j - a_i \le k$. Cấu trúc đơn điệu, vì vậy chúng ta có thể sử dụng cửa sổ trượt: khi chúng ta di chuyển con trỏ bên phải, chúng ta đảm bảo con trỏ bên trái nằm trong phạm vi mà kết nối được duy trì. Điều tốt nhất chúng ta có thể làm là giữ cửa sổ lớn nhất trong đó khoảng cách tối đa giữa các phần tử được giữ liên tiếp là ≤$k$. 

Điều này làm giảm việc nhóm mảng đã sắp xếp thành các phân đoạn liền kề trong đó các chênh lệch liền kề là ≤$k$. Bên trong mỗi phân đoạn, chúng ta có thể giữ tất cả các phần tử. Giữa các phân đoạn, không có phần tử nào có thể thu hẹp khoảng cách nên chúng độc lập. 

Như vậy câu trả lời là$n - \max(\text{size of a segment where consecutive differences} \le k)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n \log n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$mỗi bài kiểm tra |$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp dãy độ khó theo thứ tự không giảm. 

Việc sắp xếp là cần thiết vì sự kề cận trong một sự sắp xếp tối ưu sẽ luôn tôn trọng thứ tự tăng dần; bất kỳ sự sắp xếp lại nào trộn lẫn các giá trị cách xa nhau đều không giúp thu hẹp khoảng cách. 
2. Quét qua mảng đã sắp xếp và theo dõi khối liền kề dài nhất trong đó mỗi chênh lệch liên tiếp nhiều nhất$k$. 

Khối này đại diện cho một nhóm nơi chúng ta có thể đặt tất cả các phần tử theo thứ tự mà không vi phạm ràng buộc. 
3. Duy trì độ dài đoạn hiện tại và đặt lại bất cứ khi nào khoảng cách lớn hơn$k$xuất hiện. 

Khoảng trống lớn đóng vai trò như một rào cản cứng: không có vùng lân cận hợp lệ nào có thể vượt qua nó. 
4. Giữ độ dài đoạn tối đa nhìn thấy trong quá trình quét. 
5. Câu trả lời là$n - \text{maximum segment length}$, vì chúng tôi xóa mọi thứ bên ngoài khối tốt nhất có thể đạt được. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi sự sắp xếp hợp lệ đều phải tôn trọng thực tế là các khoảng trống lớn không thể được bắc cầu bằng các giá trị trung gian trừ khi các giá trị trung gian đó nằm giữa chúng theo thứ tự được sắp xếp. Điều này buộc bất kỳ lựa chọn hợp lệ nào phải nằm hoàn toàn trong một khu vực mà các phần tử liên tiếp khác nhau tối đa$k$. Mỗi khi khoảng cách vượt quá$k$, trình tự sẽ chia thành các thành phần độc lập không thể hợp nhất theo bất kỳ thứ tự nào. Do đó, giải pháp tối ưu giữ lại thành phần lớn nhất như vậy, vì trong nó tất cả các phần tử có thể được sắp xếp liên tiếp mà không vi phạm ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        a.sort()
        
        best = 1
        cur = 1
        
        for i in range(1, n):
            if a[i] - a[i - 1] <= k:
                cur += 1
            else:
                best = max(best, cur)
                cur = 1
        
        best = max(best, cur)
        
        print(n - best)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên sắp xếp mảng sao cho tất cả các mối quan hệ kề cận tiềm năng trở thành cục bộ. Sau đó, nó thực hiện quét tuyến tính đơn, nhóm các phần tử thành các phân đoạn tối đa trong đó mỗi chênh lệch liền kề đều nằm trong ngưỡng cho phép.$k$. Biến`cur`theo dõi độ dài đoạn hiện tại và`best`theo dõi phân khúc lớn nhất như vậy gặp phải. 

Chi tiết tinh tế duy nhất là bản cập nhật cuối cùng sau khi vòng lặp kết thúc, vì phân đoạn cuối cùng có thể là phân đoạn lớn nhất và nếu không sẽ bị bỏ qua. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 1
a = [1, 2, 4, 5, 6]
```Sau khi sắp xếp (đã sắp xếp), chúng tôi quét: 

| tôi | một [tôi] | khác biệt | cur | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | 1 | 1 | 
| 1 | 2 | 1 | 2 | 1 | 
| 2 | 4 | 2 | 1 | 2 | 
| 3 | 5 | 1 | 2 | 2 | 
| 4 | 6 | 1 | 3 | 2 | 

Phân đoạn tốt nhất cuối cùng là 3, tương ứng với$[4,5,6]$. 

Câu trả lời là$5 - 3 = 2$. 

Điều này cho thấy các giá trị nhỏ bị cô lập như 1 và 2 sẽ không hữu ích nếu chúng bị ngăn cách bởi một khoảng cách cấm. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 2
a = [2, 4, 6, 8]
```| tôi | một [tôi] | khác biệt | cur | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | - | 1 | 1 | 
| 1 | 4 | 2 | 2 | 1 | 
| 2 | 6 | 2 | 3 | 2 | 
| 3 | 8 | 2 | 4 | 3 | 

Tất cả các phần tử tạo thành một chuỗi hợp lệ duy nhất, vì vậy tốt nhất = 4 và câu trả lời = 0. 

Điều này xác nhận rằng khi tất cả các khoảng trống liên tiếp đều nằm trong$k$, không cần loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Việc sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm, quá trình quét là tuyến tính | 
| Không gian |$O(1)$thêm | Chỉ các bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Tổng cộng$n$trên các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó việc sắp xếp và quét vẫn nằm trong giới hạn. Thuật toán đủ hiệu quả cho các đầu vào trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import builtins
    
    input = sys.stdin.readline
    
    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, k = map(int, input().split())
            a = list(map(int, input().split()))
            a.sort()
            best = 1
            cur = 1
            for i in range(1, n):
                if a[i] - a[i - 1] <= k:
                    cur += 1
                else:
                    best = max(best, cur)
                    cur = 1
            best = max(best, cur)
            out.append(str(n - best))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""7
5 1
1 2 4 5 6
1 2
10
8 3
17 3 1 20 12 5 17 12
4 2
2 4 6 8
5 3
2 3 19 10 8
3 4
1 10 5
8 1
8 3 1 4 5 10 7 3
""") == """2
0
5
0
3
1
4"""

# all equal
assert run("""1
5 10
7 7 7 7 7
""") == "0"

# single element
assert run("""1
1 100
42
""") == "0"

# large gap splitting
assert run("""1
6 2
1 2 3 10 11 12
""") == "0"

# chain break
assert run("""1
5 1
1 3 5 7 9
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | không cần xóa | 
| phần tử đơn | 0 | trường hợp cơ sở | 
| chia thành hai khối hoàn hảo | 0 | chỉ có vấn đề khối lớn nhất | 
| khoảng cách xen kẽ nghiêm ngặt | 4 | trường hợp phân mảnh tối đa | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, việc sắp xếp sẽ không tạo ra sự khác biệt nào ở mọi nơi, do đó quá trình quét tạo thành một đoạn có độ dài duy nhất$n$. Thuật toán giữ`best = n`, dẫn đến đầu ra 0, phù hợp với thực tế là mọi thứ tự đều đã được cân bằng. 

Khi$n = 1$, vòng lặp không bao giờ chạy, và`best`vẫn là 1. Câu trả lời trở thành 0, phù hợp với một vòng hợp lệ tầm thường. 

Khi mảng chia thành hai cụm cách nhau một khoảng lớn, chẳng hạn như$[1,2,3,100,101,102]$với nhỏ$k$, quá trình quét tạo ra hai phân đoạn. Thuật toán chỉ giữ chính xác giá trị lớn hơn vì không thể có sự liền kề qua khoảng trống. 

Khi các giá trị xen kẽ với các khoảng trống luôn lớn hơn$k$, mọi phần tử sẽ trở thành phân đoạn riêng của nó. Kích thước phân đoạn tối đa là 1, vì vậy câu trả lời trở thành$n-1$, phản ánh rằng chỉ có một vấn đề có thể được giữ lại.
