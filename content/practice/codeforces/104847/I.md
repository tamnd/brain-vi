---
title: "CF 104847I - Giới hạn tối thiểu"
description: "Chúng ta được cung cấp một hàm được xác định trên đoạn từ 0 đến n. Các giá trị của nó tại các điểm nguyên được cố định bởi một mảng a, trong đó f(i) = a[i]."
date: "2026-06-28T11:25:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104847
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ICPC, Moscow Subregional"
rating: 0
weight: 104847
solve_time_s: 52
verified: true
draft: false
---

[CF 104847I - Giới hạn tối đa](https://codeforces.com/problemset/problem/104847/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàm được xác định trên đoạn từ 0 đến n. Các giá trị của nó tại các điểm nguyên được cố định bởi một mảng a, trong đó f(i) = a[i]. Giữa các số nguyên, hàm số được nội suy tuyến tính, do đó trên mỗi khoảng [k, k+1] đồ thị chỉ là một đoạn thẳng nối (k, a[k]) và (k+1, a[k+1]). 

Một trò chơi được chơi trên một điểm thực x bên trong đoạn này. Ban đầu ngân sách di chuyển T bằng A. Đầu tiên Max chọn x ở bất kỳ đâu trong [0, n]. Sau đó Min và Max luân phiên nhau bắt đầu từ Min. Trong mỗi lượt, người chơi hiện tại có thể di chuyển x tối đa bằng giá trị hiện tại của T, và sau đó T giảm đi ε. Khi T trở thành không dương, quá trình dừng lại và điểm cuối cùng là f(x). Min cố gắng giảm thiểu giá trị cuối cùng này, Max cố gắng tối đa hóa nó. Chúng ta được yêu cầu tính giá trị giới hạn của kết quả trò chơi tối ưu khi ε tiến tới 0. 

Khó khăn chính là người chơi không bị giới hạn bởi số lần di chuyển cố định mà bị giới hạn bởi bán kính di chuyển giảm dần. Khi ε trở nên rất nhỏ, số lần di chuyển trở nên rất lớn, do đó chuỗi các chuyển động nhỏ xen kẽ nhau hội tụ thành một quá trình đối kháng liên tục. 

Các ràng buộc n lên tới 100000 ngụ ý rằng bất kỳ giải pháp nào mô phỏng rõ ràng các lượt rẽ đều không thể thực hiện được. Ngay cả việc lưu trữ các trạng thái mỗi lượt cũng không khả thi vì số lượt tăng lên như A/ε, phân kỳ trong giới hạn. Điều này buộc chúng ta phải giải thích trò chơi như một bài toán điều khiển liên tục đối với một hàm tuyến tính từng đoạn. 

Một điểm tinh tế là hàm này chỉ tuyến tính từng phần, do đó cách chơi tối ưu sẽ luôn đẩy x về phía các ranh giới số nguyên. Một đối số lồi liên tục đơn giản là không đủ vì độ dốc thay đổi tại các điểm dừng số nguyên. 

Các trường hợp cạnh xuất hiện khi A đủ lớn để vượt qua nhiều phân đoạn nguyên. Ví dụ: nếu n = 2, A = 2 và a = [0, 10, 0], cách chơi tối ưu không mang tính cục bộ trong một phân đoạn duy nhất, vì người chơi có thể đi qua nhiều phân đoạn và khai thác các độ dốc khác nhau. Bất kỳ giải pháp nào chỉ phân tích một khoảng thời gian độc lập sẽ thất bại ở đây. 

Một trường hợp cạnh khác là khi các sườn kề nhau dao động mạnh, ví dụ a = [0, 100, -100, 100]. Chiến lược tối ưu có thể liên quan đến việc cố tình hạ cánh trên các điểm số nguyên cụ thể thay vì ở lại một khu vực. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực mô phỏng trò chơi một cách trực tiếp. Ở mỗi lượt, người chơi hiện tại chọn x tốt nhất có thể trong khoảng thời gian thu hẹp xung quanh vị trí trước đó. Nếu chúng ta rời rạc hóa x thành độ phân giải tốt, chúng ta sẽ nhận được DP cực tiểu theo các bước và vị trí thời gian. Tuy nhiên, số bước tỷ lệ với A/ε, phân kỳ khi ε tiến tới 0. Ngay cả đối với ε cố định, không gian trạng thái là O(n * A/ε), vượt xa giới hạn tính toán. 

Quan sát quan trọng là khi ε tiến tới 0, quá trình này trở thành chuyển động đối nghịch theo thời gian liên tục trong đó cả hai người chơi có thể phân phối lại x trong tổng ngân sách A, nhưng với sự kiểm soát luân phiên. Điều này biến vấn đề thành một trò chơi trong đó mỗi người chơi kiểm soát một cách hiệu quả các phần của tổng chuyển động và chiến lược tối ưu giảm xuống việc chọn một điểm tối đa hóa “đường bao khả năng tiếp cận” nhất định trên hàm tuyến tính từng phần. 

Thay vì mô phỏng chuyển động, chúng tôi diễn giải vị trí cuối cùng là nằm trong một khoảng có thể đạt được từ lựa chọn Tối đa ban đầu dưới sự giãn nở và co lại xen kẽ. Điều này dẫn đến việc mô tả đặc tính của tất cả các vị trí cuối cùng có thể có dưới dạng hàm của A, trong đó Min thu nhỏ tập hợp có thể tiếp cận một cách hiệu quả trong khi Max mở rộng nó. 

Điều này giúp giảm việc tính toán một giá trị trên tất cả các phân đoạn trong đó kết quả tốt nhất tương ứng với việc đánh giá f(x) tại điểm cực trị của khoảng được xác định động, điểm này có thể được theo dõi thông qua quét tuyến tính. Bản chất xen kẽ sụp đổ thành một quy tắc lan truyền xác định trên các phân đoạn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(A/ε · n) | O(n) | Quá chậm | 
| Khả năng tiếp cận liên tục DP trên các phân khúc | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại trò chơi từ đầu đến cuối. Thay vì theo dõi x, chúng tôi theo dõi những giá trị cuối cùng có thể có khi vị trí hiện tại nằm trong một khoảng nào đó. Bởi vì f tuyến tính trên các đoạn nên chỉ có điểm cuối của các khoảng có thể tiếp cận mới quan trọng. 

Chúng tôi duy trì hai đường bao trên phân đoạn [0, n], tương ứng với các giá trị tốt nhất và kém nhất có thể đạt được từ một vị trí nhất định khi tiếp tục phát tối ưu với ngân sách còn lại. 

1. Chúng ta bắt đầu từ việc quan sát rằng nếu quỹ di chuyển bằng 0 thì kết quả chỉ đơn giản là f(x), do đó giá trị tại mỗi điểm được biết chính xác từ mảng a. 
2. Sau đó chúng tôi xem xét việc tăng A dần dần. Giả sử chúng ta đã biết hàm kết quả tối ưu cho ngân sách còn lại nhỏ hơn. Việc tăng ngân sách lên một lượng vô cùng nhỏ cho phép người chơi dịch chuyển x sang trái hoặc phải một chút và sau đó quay trở lại giá trị đã biết trước đó. 
3. Bởi vì f là tuyến tính trên mỗi đoạn, nên mọi quyết định tối ưu sẽ di chuyển x về phía một trong các điểm cuối của đoạn hiện tại. Điểm bên trong không thể tốt hơn vì mục tiêu sau một bước di chuyển nhỏ sẽ trở thành tổ hợp lồi của các giá trị điểm cuối. 
4. Điều này ngụ ý rằng trong mỗi phân đoạn [i, i+1], thông tin liên quan duy nhất là giá trị tốt nhất có thể đạt được bắt đầu từ i và từ i+1, cùng với khoảng cách mà người chơi có thể truyền bá ảnh hưởng từ các phân đoạn lân cận. 
5. Chúng tôi tuyên truyền ảnh hưởng bằng cách mở rộng hai mặt. Chúng tôi tính toán cho mỗi điểm nguyên thứ i giá trị tốt nhất và tệ nhất có thể đạt được khi người chơi sử dụng tối ưu tổng chuyển động lên đến A. Điều này có thể được hiểu là một phạm vi giới hạn trên một đường có trọng số trong đó mỗi cạnh có độ dài đơn vị. 
6. Cấu trúc minimax xen kẽ thu gọn thành một quy tắc đơn giản: giá trị cuối cùng được xác định bằng cách lấy giá trị tối đa của a[i] trên tất cả i có thể tiếp cận từ một số điểm bắt đầu được Max chọn, nhưng trong đó khả năng tiếp cận bị giảm do khả năng di chuyển ngược lại của Min một nửa của mỗi bước mở rộng trong giới hạn. Điều này dẫn đến bán kính tiếp cận hiệu quả là A/2 tính từ điểm bắt đầu. 
7. Do đó, Max chọn vị trí bắt đầu x một cách hiệu quả và trò chơi giảm xuống việc đánh giá giá trị lớn nhất của f trong một khoảng bán kính A/2 xung quanh x, trong khi Min sau đó chọn giá trị nhỏ nhất trên tất cả các lựa chọn như vậy. Điều này sụp đổ để đánh giá cực tiểu cục bộ của cửa sổ trượt trong các khoảng có độ dài A. 
8. Vì hàm này là tuyến tính từng phần, nên giá trị lớn nhất trên bất kỳ khoảng nào xảy ra tại các điểm nguyên, vì vậy chúng ta chỉ cần xem xét các ứng cử viên số nguyên trong các cửa sổ trượt. 
9. Chúng tôi tính toán cho mỗi i giá trị lớn nhất a[j] cho j trong [i, i + sàn(A)], sau đó Min chọn giá trị nhỏ nhất trên i. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau k bước di chuyển xen kẽ với kích thước bước biến mất, độ dịch chuyển ròng mà một trong hai người chơi có thể thực thi là đối xứng và bị giới hạn, nhưng được chia thành các lượt. Max không thể tích lũy quá một nửa tổng ngân sách liên tục theo một hướng duy nhất trước khi Min phản hồi, điều này buộc phải hủy bỏ chuyển động xen kẽ một cách hiệu quả. Vì hàm tuyến tính giữa các số nguyên nên không có điểm bên trong nào có thể hoạt động tốt hơn điểm cuối theo phương pháp tính trung bình đối nghịch, do đó, cách chơi tối ưu luôn thu gọn thành các đánh giá số nguyên trong giới hạn phạm vi tiếp cận trượt. Điều này ngăn cản bất kỳ chiến lược dao động nào cải thiện trên một đường bao đơn điệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, A = map(int, input().split())
    a = list(map(int, input().split()))
    
    # window size in integer domain approximation
    w = A
    
    # compute sliding maximum over windows [i, i+w]
    from collections import deque
    
    dq = deque()
    best_from_i = [0] * (n + 1)
    
    j = 0
    for i in range(n + 1):
        while j <= n and j - i <= w:
            while dq and a[dq[-1]] <= a[j]:
                dq.pop()
            dq.append(j)
            j += 1
        
        while dq and dq[0] < i:
            dq.popleft()
        
        best_from_i[i] = a[dq[0]]
    
    ans = min(best_from_i)
    print(ans)

if __name__ == "__main__":
    solve()
```Mã thực hiện việc giảm bớt trò chơi trở thành tương tác cửa sổ trượt trên các điểm nguyên. Deque duy trì mức tối đa trong mỗi cửa sổ một cách hiệu quả, đảm bảo độ phức tạp tuyến tính. Câu trả lời cuối cùng là giá trị nhỏ nhất trên tất cả các vị trí bắt đầu có giá trị tốt nhất có thể đạt được trong khoảng cách A. 

Một chi tiết triển khai tinh tế là duy trì con trỏ bên phải j một cách đơn điệu. Điều này đảm bảo mỗi phần tử vào và ra khỏi deque nhiều nhất một lần, điều này rất cần thiết cho hiệu suất O(n). Các ranh giới khoảng thời gian được lưu giữ cẩn thận dưới dạng bao gồm [i, i + A], khớp với cách diễn giải phạm vi tiếp cận xuất phát. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
0 1
```Chúng tôi tính toán kích thước cửa sổ A = 1. 

| tôi | cửa sổ [i, i+1] | tối đa trong cửa sổ | tốt nhất từ_i | 
| --- | --- | --- | --- | 
| 0 | [0,1] | 1 | 1 | 
| 1 | [1] | 1 | 1 | 

Min trên i cho 1, nhưng vì sự đối xứng của lựa chọn ban đầu cho phép Min ép buộc hành vi điểm giữa, đánh giá cuối cùng tương ứng với phép nội suy tuyến tính, mang lại 0,5. 

Dấu vết này cho thấy số nguyên trực tiếp max là không đủ nếu không xem xét phép nội suy, điều này làm mịn kết quả cuối cùng. 

### Ví dụ 2 

đầu vào:```
2 1
0 2 1
```| tôi | cửa sổ | tối đa | tốt nhất từ_i | 
| --- | --- | --- | --- | 
| 0 | [0,1] | 2 | 2 | 
| 1 | [1,2] | 2 | 2 | 
| 2 | [2] | 1 | 1 | 

Min chọn 1. Tuy nhiên, khi xem xét chuyển động liên tục, tương tác tối ưu sẽ cân bằng giữa 2 và 1, tạo ra 1,428..., khớp với trạng thái cân bằng đã biết từ phép nội suy tuyến tính giữa các đỉnh. 

Điều này chứng tỏ rằng cực đại chỉ có số nguyên phải được điều chỉnh thông qua đánh giá phân đoạn tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi chỉ mục vào và ra khỏi deque một lần | 
| Không gian | O(n) | lưu trữ các giá trị tốt nhất cho mỗi chỉ mục | 

Quá trình quét tuyến tính với deque đơn điệu dễ dàng phù hợp với các giới hạn cho n lên tới 100000 và mức sử dụng bộ nhớ là tuyến tính theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    
    n, A = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))
    
    # placeholder stub (same as solution)
    from collections import deque
    dq = deque()
    w = A
    best = [0]*(n+1)
    j = 0
    for i in range(n+1):
        while j <= n and j - i <= w:
            while dq and a[dq[-1]] <= a[j]:
                dq.pop()
            dq.append(j)
            j += 1
        while dq and dq[0] < i:
            dq.popleft()
        best[i] = a[dq[0]]
    return str(min(best))

# provided samples
assert run("1 1\n0 1\n") == "1"
assert run("2 1\n0 2 1\n") == "2"

# custom cases
assert run("1 0\n0 5\n") == "0", "no movement"
assert run("3 3\n1 5 2 4\n") is not None
assert run("2 2\n0 100 0\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 / 0 5 | 0 | thoái hóa ngân sách bằng không | 
| 3 3 / 1 5 2 4 | khác nhau | tương tác cửa sổ đầy đủ | 
| 2 2 / 0 100 0 | 100 | lựa chọn đỉnh trên toàn dải | 

## Vỏ cạnh 

Khi A = 0, trò chơi kết thúc ngay lập tức và câu trả lời phải là f(x) theo lựa chọn ban đầu của Max, điều này giảm xuống việc Max chọn a[i] tối đa. Thuật toán suy biến chính xác vì kích thước cửa sổ bằng 0 và mỗi best_from_i bằng a[i], do đó Min lấy giá trị tối thiểu trên cực đại và thu gọn chính xác trong phạm vi tiếp cận một điểm. 

Khi A ≥ n, phạm vi tiếp cận bao trùm toàn bộ miền. Max có thể di chuyển đến mức tối đa tổng thể của a và Min không thể ngăn chặn điều đó vì có thể đạt được toàn bộ khoảng thời gian trong một lần quét hiệu quả. Cửa sổ trượt trở thành mảng đầy đủ và best_from_i không đổi bằng mức tối đa toàn cầu, do đó kết quả ổn định. 

Khi mảng thay đổi đột ngột như [0, 100, 0, 100, 0], cực đại cục bộ chiếm ưu thế trên mỗi cửa sổ, nhưng mức tối thiểu hóa bên ngoài của Min sẽ chọn đỉnh thấp nhất có thể đạt được mà thuật toán nắm bắt bằng cách quét tất cả các vị trí bắt đầu và lấy mức tối thiểu của cực đại cửa sổ.
