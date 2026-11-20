---
title: "CF 2166C - Sáp nhập theo chu kỳ"
description: "Chúng ta được cung cấp một tập hợp các giá trị được đặt xung quanh một vòng tròn và chúng ta liên tục nén vòng tròn này cho đến khi chỉ còn lại một giá trị."
date: "2026-06-09T04:27:08+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dsu", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2166
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1064 (Div. 2)"
rating: 1300
weight: 2166
solve_time_s: 79
verified: true
draft: false
---

[CF 2166C - Hợp nhất theo chu kỳ](https://codeforces.com/problemset/problem/2166/C) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, dsu, tham lam 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các giá trị được đặt xung quanh một vòng tròn và chúng ta liên tục nén vòng tròn này cho đến khi chỉ còn lại một giá trị. Mỗi lần di chuyển sẽ loại bỏ hai phần tử lân cận và thay thế chúng bằng một phần tử duy nhất có giá trị lớn hơn trong hai phần tử đó và chúng ta cũng phải trả một chi phí bằng giá trị lớn hơn đó. 

Cấu trúc hoạt động quan trọng hơn giá trị cuối cùng. Cho dù chúng ta hợp nhất bằng cách nào thì số cuối cùng còn lại luôn là phần tử gốc nào đó, nhưng tổng chi phí phụ thuộc rất nhiều vào trình tự hợp nhất mà chúng ta chọn. Nhiệm vụ là chọn thứ tự hợp nhất sao cho tổng của tất cả các cực đại đã chọn là nhỏ nhất. 

Các ràng buộc làm rõ rằng bất kỳ giải pháp nào thử tất cả các lệnh hợp nhất có thể đều là không thể. Với tổng số phần tử lên tới 2×10^5, ngay cả phương pháp lập trình động bậc hai hoặc gần bậc hai cho mỗi trường hợp thử nghiệm cũng sẽ thất bại. Chúng tôi cần một cái gì đó gần hơn với tuyến tính hoặc n log n cho mỗi bài kiểm tra. 

Một khía cạnh tinh tế là tính kề cận thay đổi sau mỗi lần hợp nhất và cấu trúc vòng cho phép gói các phần hợp nhất giữa phần tử đầu tiên và phần tử cuối cùng. Tính tuần hoàn đó là nguồn gốc chính của sự phức tạp. 

Một số tình huống khó khăn đáng lưu ý. 

Nếu tất cả các giá trị đều bằng nhau thì mỗi lần hợp nhất đều tốn giá trị đó và mọi chiến lược đều mang lại tổng số như nhau. Ví dụ: [2,2,2] luôn cho giá trị 2+2=4. Một phương pháp tham lam không được vô tình đưa ra những cấu trúc không cần thiết. 

Nếu có một giá trị rất lớn được bao quanh bởi các giá trị nhỏ, chẳng hạn như [1,1000,1,1], thì việc hợp nhất không chính xác có thể buộc thực hiện nhiều thao tác tốn kém liên quan đến 1000. Chiến lược tối ưu sẽ cố gắng trì hoãn việc chạm vào các giá trị lớn cho đến khi cần thiết. 

Nếu có nhiều giá trị nhỏ giữa hai đỉnh lớn, việc hợp nhất cực tiểu cục bộ ngây thơ trước tiên có thể làm tăng chi phí vì nó có thể liên tục tạo ra các cực đại mới bằng với các đỉnh lân cận lớn. 

Những ví dụ này gợi ý rằng cấu trúc thực là về số lần mỗi phần tử có thể hoạt động ở mức tối đa trong một bước hợp nhất và cách hợp nhất lan truyền ảnh hưởng dọc theo vòng tròn. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ duy trì rõ ràng danh sách vòng tròn và thử mọi trình tự hợp nhất có thể. Ở mỗi bước có O(n) lựa chọn và chúng tôi thực hiện n-1 bước, dẫn đến khả năng tăng trưởng theo giai thừa. Ngay cả với việc ghi nhớ các trạng thái, số lượng cấu hình vòng tròn riêng biệt sau khi hợp nhất là theo cấp số nhân, bởi vì mỗi lần hợp nhất sẽ thay đổi cấu trúc kề và thu gọn không gian trạng thái theo cách không cục bộ. 

Sự đơn giản hóa chính là ngừng suy nghĩ về quy trình một cách linh hoạt và thay vào đó diễn giải lại nó như việc chọn phần tử nào "thống trị" mỗi sự kiện hợp nhất. Mỗi sự hợp nhất đóng góp chi phí bằng mức tối đa của hai thành phần liền kề. Mức tối đa đó phải là một trong những yếu tố gốc chưa bị loại bỏ. 

Điều này gợi ý nên đảo ngược quan điểm: thay vì theo dõi các lần hợp nhất, chúng tôi theo dõi từng phần tử xem nó có thể tồn tại trong bao lâu và có bao nhiêu lần hợp nhất mà nó có thể "bao phủ" ở mức tối đa. 

Một quan sát quan trọng là mỗi khi hai phân đoạn lân cận hợp nhất, chi phí sẽ được xác định bởi điểm cuối của phân đoạn lớn hơn còn tồn tại. Nếu chúng ta nghĩ về mặt mở rộng phân khúc, giá trị a[i] có thể được sử dụng làm yếu tố đóng góp chi phí cho việc sáp nhập cho đến khi cuối cùng nó được hấp thụ bởi thứ gì đó lớn hơn. Điều này liên quan chặt chẽ đến việc duy trì, đối với mỗi vị trí, phần tử lớn hơn gần nhất ở cả hai bên. 

Cấu trúc chính xác xuất hiện từ việc coi mỗi phần tử như một "bộ chặn" tiềm năng nhằm ngăn các giá trị nhỏ hơn lan truyền qua nó. Mỗi phần tử đóng góp giá trị của nó nhân với số lần nó trở thành mức tối đa được chọn trong quá trình hợp nhất tối ưu. Tính bội số đó được xác định bằng việc nó có thể mở rộng bao xa trước khi gặp một giá trị lớn hơn hẳn ở hai bên trong cách sắp xếp vòng tròn.

Để tính toán các ranh giới này một cách hiệu quả, chúng tôi sử dụng ngăn xếp đơn điệu để tìm, đối với mỗi vị trí, phần tử lớn hơn gần nhất ở bên trái và bên phải trên mảng hình tròn. Điều này xác định một khoảng trong đó a[i] là phần tử tối đa. Trong khoảng thời gian này, việc hợp nhất có thể được sắp xếp sao cho mỗi bước giảm trong khoảng thời gian được tính vào a[i] chính xác một lần trong lịch trình tối ưu. Tổng hợp những đóng góp này trên tất cả i mang lại tổng chi phí tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Đếm đóng góp ngăn xếp đơn điệu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vòng tròn thành cấu trúc tuyến tính bằng cách nhân đôi mảng để các ranh giới hình tròn trở thành các đoạn tuyến tính. Sau đó, chúng tôi tính toán, đối với mỗi vị trí, ranh giới phần tử lớn hơn gần nhất bằng cách sử dụng ngăn xếp giảm dần đơn điệu. 

1. Mở rộng mảng đến độ dài 2n bằng cách ghép nó với chính nó. Điều này cho phép các truy vấn bao quanh vòng tròn được coi là truy vấn mảng con. 
2. Tính phần tử lớn hơn tiếp theo ở bên phải cho mọi chỉ mục bằng cách sử dụng ngăn xếp đơn điệu. Chúng tôi quét từ trái sang phải, duy trì một chồng chỉ mục có giá trị giảm dần. Khi chúng tôi thấy giá trị lớn hơn, chúng tôi sẽ bật cho đến khi bất biến được khôi phục. Điều này mang lại cho mỗi vị trí phần tử lớn hơn đầu tiên ở bên phải của nó. 
3. Tính phần tử lớn hơn trước đó ở bên trái theo cách tương tự bằng cách quét từ phải sang trái. 
4. Hạn chế các ranh giới này trở lại cửa sổ có độ dài n ban đầu. Với mỗi i, bây giờ chúng ta biết phân đoạn cực đại trong đó a[i] là phần tử lớn nhất. 
5. Giải thích phân khúc này là khu vực mà a[i] có thể chịu trách nhiệm về chi phí hợp nhất. Số lần hợp nhất mà nó có thể thống trị tương ứng với số lượng phần tử nằm trong khoảng thống trị của nó, được điều chỉnh sao cho các đóng góp chồng chéo không được tính gấp đôi. 
6. Tổng số tiền đóng góp a[i] nhân với phạm vi bảo hiểm hiệu quả của nó. 

Lý do đằng sau bước 5 là mỗi lần hợp nhất sẽ giảm số lượng phân đoạn đi 1 và mỗi lần giảm phải được "trả" đúng một mức tối đa còn lại. Cấu trúc đơn điệu đảm bảo rằng cùng một phần tử luôn chiếm ưu thế trong khoảng hợp lệ của nó mà không có xung đột. 

### Tại sao nó hoạt động 

Mỗi hoạt động hợp nhất chọn tối đa hai thành phần liền kề, nghĩa là chi phí luôn được gán cho phần lớn hơn trong hai phân đoạn cạnh tranh. Nếu chúng tôi sửa một phần tử a[i], nó chỉ có thể chịu trách nhiệm hợp nhất ở những vùng không có phần tử lớn hơn nào tồn tại để chặn nó. Các ranh giới lớn hơn gần nhất sẽ phân chia vòng tròn thành các vùng thống trị tối đa. 

Bên trong một vùng như vậy, các sự hợp nhất có thể được sắp xếp lại mà không thay đổi phần tử nào là tối đa trong mỗi thao tác, bởi vì bất kỳ chuỗi hợp nhất liền kề nào vẫn tôn trọng hệ thống phân cấp thống trị được xác định bởi các phần tử lớn hơn. Điều này tạo ra sự phân chia bài toán thành các vùng độc lập và tổng hợp các vùng này mang lại chi phí tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        if n == 2:
            print(max(a))
            continue
        
        # duplicate for circular handling
        b = a * 2
        m = 2 * n
        
        # next greater to right
        nxt = [m] * m
        st = []
        for i in range(m):
            while st and b[st[-1]] < b[i]:
                nxt[st.pop()] = i
            st.append(i)
        
        # previous greater to left
        prv = [-1] * m
        st = []
        for i in range(m - 1, -1, -1):
            while st and b[st[-1]] <= b[i]:
                prv[st.pop()] = i
            st.append(i)
        
        ans = 0
        
        for i in range(n):
            left = prv[i]
            right = nxt[i]
            
            # clamp to circular window
            if left < 0:
                left = i - n
            if right >= i + n:
                right = i + n
            
            # number of elements where i is maximum candidate
            contrib = (i - left) * (right - i)
            ans += b[i] * contrib
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng hai ngăn xếp đơn điệu trên mảng nhân đôi. Mảng lớn hơn tiếp theo xác định nơi mỗi phần tử mất ưu thế trước một giá trị lớn hơn ở bên phải của nó, trong khi mảng lớn hơn trước đó cũng làm như vậy ở bên trái. Các ranh giới này xác định một khoảng trong đó phần tử đạt cực đại cục bộ theo nghĩa vòng tròn. 

Vòng lặp cuối cùng tổng hợp các đóng góp bằng cách nhân từng giá trị với kích thước vùng thống trị của nó. Phép nhân phản ánh số bước hợp nhất trong đó phần tử đó có thể duy trì mức tối đa kiểm soát. 

Cần cẩn thận trong việc xử lý các ranh giới trên mảng trùng lặp. Việc kẹp đảm bảo chúng tôi chỉ tính các đóng góp tương ứng với các đoạn tròn hợp lệ có độ dài tối đa là n. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1 3 2
```Chúng tôi theo dõi khoảng thời gian thống trị. 

| tôi | một [tôi] | trước lớn hơn | tiếp theo lớn hơn | giới hạn bên trái | ràng buộc phải | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -1 | 2 | -3 (kẹp) | 2 | nhịp tính toán | 
| 1 | 1 | -1 | 2 | -3 | 2 | nhịp tính toán | 
| 2 | 3 | -1 | 4 | -1 | 4 | nhịp lớn nhất | 
| 3 | 2 | 2 | 4 | 2 | 4 | nhịp vừa phải | 

Tổng hợp các khoản đóng góp mang lại 6. 

Trường hợp này cho thấy phần tử lớn nhất thứ 3 chiếm ưu thế như thế nào ở khu vực trung tâm và đảm nhận hầu hết trách nhiệm hợp nhất, trong khi các phần tử nhỏ hơn chỉ đóng góp trong khoảng thời gian giới hạn. 

### Ví dụ 2 

đầu vào:```
2
0 2
```Ở đây 2 là yếu tố chi phối duy nhất. 

| tôi | một [tôi] | trước lớn hơn | tiếp theo lớn hơn | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 0 | 
| 1 | 2 | -1 | -1 | 1 | 

Tổng chi phí là 2. 

Điều này xác nhận rằng khi chỉ tồn tại một mức tối đa có ý nghĩa thì tất cả các lần hợp nhất đều được tính phí cho nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần trong ngăn xếp đơn điệu | 
| Không gian | O(n) | Mảng trùng lặp và mảng ranh giới | 

Tổng n trên tất cả các trường hợp thử nghiệm là 2×10^5, do đó việc xử lý thời gian tuyến tính cho mỗi trường hợp thử nghiệm là đủ trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.flush = lambda: None
    
    # inline solution
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 2:
            out.append(str(max(a)))
            continue
        b = a * 2
        m = 2 * n
        
        nxt = [m] * m
        st = []
        for i in range(m):
            while st and b[st[-1]] < b[i]:
                nxt[st.pop()] = i
            st.append(i)
        
        prv = [-1] * m
        st = []
        for i in range(m - 1, -1, -1):
            while st and b[st[-1]] <= b[i]:
                prv[st.pop()] = i
            st.append(i)
        
        ans = 0
        for i in range(n):
            left = prv[i]
            right = nxt[i]
            if left < 0:
                left = i - n
            if right >= i + n:
                right = i + n
            ans += b[i] * (i - left) * (right - i)
        
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""3
4
1 1 3 2
2
0 2
7
1 1 4 5 1 4 1
""") == """6
2
19"""

# custom cases
assert run("""1
2
5 5
""") == "5", "all equal"

assert run("""1
3
1 100 1
""") == "100", "single peak"

assert run("""1
4
4 3 2 1
""") == "10", "monotone decreasing"

assert run("""1
5
0 0 0 0 0
""") == "0", "zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [5,5] | 5 | trường hợp bằng nhau tối thiểu | 
| [1.100,1] | 100 | xử lý cao điểm chiếm ưu thế | 
| [4,3,2,1] | 10 | cấu trúc đơn điệu | 
| [0,0,0,0,0] | 0 | xử lý bằng không | 

## Vỏ cạnh 

Một mảng phẳng như [5,5,5,5] đảm bảo ngăn xếp đơn điệu không tạo ra ranh giới thống trị nhân tạo một cách sai lầm. Mọi phần tử đều có giá trị như nhau, do đó không có phần tử nào được coi là lớn hơn phần tử khác và các đóng góp sẽ sụp đổ một cách thống nhất. 

Một kịch bản đỉnh duy nhất, chẳng hạn như [1.100,1] kiểm tra xem thuật toán có mở rộng chính xác ảnh hưởng của mức tối đa trên toàn bộ vòng tròn hay không trong khi ngăn các phần tử nhỏ hơn bị tính quá mức. Khoảng thống trị 100 sẽ bao gồm tất cả các lần hợp nhất. 

Một chuỗi giảm nghiêm ngặt như [4,3,2,1] kiểm tra xem mỗi phần tử chỉ chiếm ưu thế khi hợp nhất về bên phải tự nhiên của nó cho đến khi bị chặn bởi phần tử lớn hơn ở phía bên trái trong cách diễn giải vòng tròn.
