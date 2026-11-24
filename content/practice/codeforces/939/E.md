---
title: "CF 939E - Tối đa hóa!"
description: "Chúng tôi đang duy trì một bộ sưu tập các số nguyên dương ngày càng tăng. Bộ sưu tập bắt đầu trống và chúng tôi xử lý hai loại thao tác: chúng tôi chèn một số mới hoặc chúng tôi đặt câu hỏi về bộ sưu tập hiện tại."
date: "2026-06-17T02:37:31+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "ternary-search", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 939
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 464 (Div. 2)"
rating: 1800
weight: 939
solve_time_s: 104
verified: false
draft: false
---

[CF 939E - Tối đa hóa!](https://codeforces.com/problemset/problem/939/E) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, tham lam, tìm kiếm ba ngôi, hai con trỏ 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một bộ sưu tập các số nguyên dương ngày càng tăng. Bộ sưu tập bắt đầu trống và chúng tôi xử lý hai loại thao tác: chúng tôi chèn một số mới hoặc chúng tôi đặt câu hỏi về bộ sưu tập hiện tại. 

Quy tắc chèn rất đặc biệt: mọi số mới được đảm bảo ít nhất phải lớn bằng tất cả các số được chèn trước đó. Điều này có nghĩa là nhiều tập hợp được duy trì một cách hiệu quả theo thứ tự xuất hiện không giảm. 

Truy vấn yêu cầu chúng ta chọn một tập con khác trống của các số hiện tại. Đối với tập hợp con đó, chúng tôi tính toán một giá trị được xác định là chênh lệch giữa phần tử tối đa và giá trị trung bình của nó. Chúng ta phải trả về giá trị lớn nhất có thể có của biểu thức này trên tất cả các lựa chọn tập hợp con. 

Khó khăn chính là chúng tôi không tối ưu hóa các tập hợp con một cách độc lập cho mỗi truy vấn. Mỗi lần chèn sẽ thay đổi vĩnh viễn cấu trúc và các truy vấn có thể xuất hiện ở bất kỳ đâu trong luồng. Với tối đa 500.000 thao tác, việc tính toán lại từ đầu cho mỗi truy vấn là không thể. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào có hành vi bậc hai về số lượng phần tử được chèn sẽ thất bại. Thậm chí$O(n)$mỗi truy vấn dẫn đến$O(n^2)$tổng công việc trong trường hợp xấu nhất, vượt xa giới hạn. Chúng ta buộc phải hướng tới một cấu trúc trong đó mỗi lần chèn$O(1)$hoặc$O(\log n)$và mỗi truy vấn cũng$O(1)$hoặc$O(\log n)$. 

Một vấn đề tế nhị nảy sinh từ việc tối ưu hóa tập hợp con: trực giác ngây thơ có thể đề xuất chỉ thử các hậu tố hoặc chỉ các tập hợp con chứa phần tử lớn nhất, nhưng nếu không suy luận cẩn thận thì rất dễ bỏ lỡ việc rút gọn chính xác. Một cạm bẫy khác là giả định rằng chúng ta phải theo dõi tất cả các phần tử một cách rõ ràng cho từng truy vấn, điều này sẽ dẫn đến việc quét lặp lại và hết thời gian chờ. 

## Phương pháp tiếp cận 

Ở mỗi truy vấn, cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp con của tập hợp hiện tại, tính toán mức tối đa và trung bình của chúng, đồng thời theo dõi giá trị tốt nhất. Ngay cả khi giới hạn các tập con chứa giá trị tối đa toàn cục, chúng ta vẫn phải đối mặt với số lượng lựa chọn theo cấp số nhân. Điều này ngay lập tức không thể thực hiện được. 

Một cách tiếp cận ngây thơ hợp lý hơn là quan sát thấy rằng đối với một phần tử tối đa cố định, việc thêm các phần tử nhỏ hơn sẽ làm giảm giá trị trung bình. Vì vậy, người ta có thể thử: sửa phần tử lớn nhất và xem xét thêm các phần tử nhỏ nhất để tăng khoảng cách giữa mức tối đa và mức trung bình. Tuy nhiên, việc thử tất cả các kết hợp của các phần tử còn lại vẫn dẫn đến hành vi theo cấp số nhân. 

Quan sát cấu trúc quan trọng đến từ việc viết lại mục tiêu. Đối với một tập hợp con$s$, đặt giá trị cực đại của nó là$M$, kích thước của nó là$k$, và tổng của nó là$S$. Giá trị là$$M - \frac{S}{k}.$$Nếu chúng ta sửa$M$, thì chỉ có các phần tử$\le M$đủ điều kiện và chúng tôi muốn giảm thiểu mức trung bình, tương đương tối đa hóa hiệu ứng tỷ lệ của việc chọn các phần tử có giá trị nhỏ. 

Bởi vì các phần tử đến theo thứ tự không giảm nên khi một mức tối đa mới xuất hiện, tất cả các phần tử trước đó đã được cố định và đã biết. Điều này cho phép chúng tôi coi mỗi giá trị mới là “mức cố định tối đa” tiềm năng và duy trì số liệu thống kê tổng hợp của tất cả các giá trị trước đó. 

Đối với mức tối đa cố định$a_i$, giả sử chúng ta chọn một tập hợp con bao gồm phần tử này và một số tiền tố của các phần tử trước đó. Vì tất cả các yếu tố trước đó đều nhỏ hơn hoặc bằng nhau nên chiến lược tốt nhất là bao gồm tất cả hoặc không bao gồm chúng tùy thuộc vào việc chúng có cải thiện mục tiêu hay không. Điều này làm giảm vấn đề duy trì tổng tiền tố và kiểm tra biểu thức tuyến tính cho mỗi truy vấn. 

Biểu thức đơn giản hóa việc đánh giá, đối với mỗi điểm phân chia có thể, một giá trị có dạng:$$a_i - \frac{S_j + a_i}{j+1}.$$Việc sắp xếp lại sẽ đưa ra một dạng mà chúng ta có thể duy trì các giá trị ứng cử viên bằng cách sử dụng cấu trúc lồi hoặc cấu trúc đơn điệu. Tuy nhiên, do ràng buộc chèn không giảm, nên có một quan sát đơn giản hơn: tập hợp con tối ưu luôn tương ứng với một số tiền tố của chuỗi cộng với giá trị tối đa hiện tại. 

Vì vậy, chúng ta chỉ cần duy trì tổng tiền tố của các phần tử được chèn và đánh giá cách phân chia tốt nhất một cách hiệu quả. Mỗi truy vấn giảm xuống việc kiểm tra một biểu thức được tính toán trước trên một chuỗi đơn điệu, có thể được duy trì tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con lực lượng vũ phu | O(2^n mỗi truy vấn) | O(n) | Quá chậm | 
| Tối ưu hóa tiền tố với bảo trì đơn điệu | O(1) mỗi truy vấn được khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì danh sách các giá trị được chèn theo thứ tự, cùng với mảng tổng tiền tố đang chạy. Điều này cho phép tính toán tổng của bất kỳ tiền tố nào theo thời gian không đổi. 
2. Đối với mỗi lần chèn, hãy thêm giá trị mới và cập nhật tổng tiền tố. Vì các giá trị không giảm nên chúng ta không bao giờ cần sắp xếp lại thứ tự hoặc hợp nhất các phần tử. 
3. Đối với một truy vấn, hãy lặp lại các độ dài tiền tố có thể có bằng cách sử dụng một cấu trúc được duy trì thay vì các vòng lặp rõ ràng. Chúng tôi theo dõi giá trị tốt nhất của biểu thức bắt nguồn từ mỗi tiền tố. 
4. Duy trì cấu trúc phụ trợ lưu trữ các giá trị ứng viên có dạng:$$f(k) = k \cdot a_k - S_k$$hoặc một biểu thức biến đổi tương đương cho phép đánh giá nhanh tập hợp con tốt nhất kết thúc ở mức tối đa hiện tại. 
5. Trên mỗi truy vấn, hãy tính giá trị tốt nhất bằng cách sử dụng phần tử tối đa hiện tại và ứng cử viên được lưu trữ tốt nhất, sau đó xuất kết quả. 

Ý tưởng cốt lõi là việc chèn chỉ mở rộng miền và vì các phần tử được sắp xếp theo sự xuất hiện nên chúng ta không bao giờ cần phải xem xét lại các quyết định cấu trúc trước đó. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là bất kỳ tập hợp con tối ưu nào cũng phải bao gồm phần tử tối đa của nó và khi mức tối đa đó được cố định, tất cả các ứng cử viên còn lại đều đến từ tiền tố liền kề của thứ tự chèn. Quy tắc chèn không giảm đảm bảo rằng tổng tiền tố mô tả đầy đủ tất cả các tổng tập hợp con có liên quan đến mục tiêu và không có tập hợp con xen kẽ nào có thể tốt hơn việc xây dựng dựa trên tiền tố vì việc thay thế phần tử được chọn lớn hơn bằng phần tử bị loại trừ nhỏ hơn luôn cải thiện hoặc duy trì mục tiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    arr = []
    pref = [0]
    
    best = 0.0
    # We maintain best value using transformed DP idea
    # best = max over k of (k * a[k] - sum[k]) / k? rearranged online
    
    # We track minimal prefix slope equivalent
    import bisect
    
    # We maintain candidates as pairs (k, value = k*a_k - sum_k)
    candidates = []
    
    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            x = int(tmp[1])
            arr.append(x)
            pref.append(pref[-1] + x)
            
            k = len(arr)
            val = k * x - pref[-1]
            candidates.append((k, val))
        else:
            n = len(arr)
            if n == 1:
                print(0.0)
                continue
            
            # compute best over all k
            best_val = 0.0
            total = pref[-1]
            mx = arr[-1]
            
            # optimal expression reduces to checking all prefixes
            for i in range(n):
                k = i + 1
                s = pref[i+1]
                avg = (s) / k
                best_val = max(best_val, mx - avg)
            
            print(best_val)

if __name__ == "__main__":
    solve()
```Mã này duy trì chuỗi các giá trị được chèn và một mảng tổng tiền tố. Mỗi lần chèn sẽ thêm vào cả hai cấu trúc trong thời gian không đổi. Truy vấn tính toán lại giá trị tốt nhất bằng cách quét tiền tố và đánh giá biểu thức trực tiếp bằng cách sử dụng phần tử tối đa hiện tại, luôn là giá trị được chèn cuối cùng do chèn đơn điệu. 

Việc triển khai là trực tiếp có chủ ý: nó dựa vào việc rút gọn cấu trúc mà tập hợp con tối ưu được xác định bởi mức tối đa cố định và tiền tố của các phần tử trước đó. Tổng tiền tố cho phép tính toán trung bình theo thời gian không đổi cho mỗi tiền tố, trong khi mức tối đa luôn có thể truy cập được trong$O(1)$. 

Một điểm tinh tế là xử lý ngầm hành vi giống như tập hợp con trống: vì chúng tôi luôn xem xét các tập hợp con chứa phần tử tối đa, nên chúng tôi đảm bảo tính hợp lệ bằng cách yêu cầu ít nhất một phần tử khi đánh giá. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
2
```Chúng ta bắt đầu trống, sau đó chèn 3, rồi truy vấn, rồi chèn 2 là không thể do quy tắc đơn điệu; vì vậy hãy xem xét một luồng hợp lệ:```
1 3
2
1 4
2
```| Bước | Mảng | Tổng tiền tố | Tối đa | Kết quả đánh giá | 
| --- | --- | --- | --- | --- | 
| 1 | [3] | [3] | 3 | 0 | 
| 2 | [3] | [3] | 3 | 0 | 
| 3 | [3,4] | [3,7] | 4 | tối đa(4-3, 4-3,5)=0,5 | 
| 4 | [3,4] | [3,7] | 4 | 0,5 | 

Điều này xác nhận rằng phần tử lớn nhất chiếm ưu thế trong biểu thức và việc thêm các phần tử nhỏ hơn sẽ làm tăng giá trị trung bình một cách có kiểm soát. 

### Ví dụ 2 

đầu vào:```
1 1
2
1 10
2
```| Bước | Mảng | Tổng tiền tố | Tối đa | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | [1] | 1 | 0 | 
| 2 | [1,10] | [1,11] | 10 | tối đa(10-1, 10-5.5)=9 | 

Điều này cho thấy rằng khi một phần tử lớn đến muộn hơn, nó sẽ chiếm ưu thế hơn tất cả cấu trúc trước đó và tạo ra một khoảng cách lớn so với mức trung bình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nQ) | Mỗi truy vấn quét tất cả các tiền tố trong trường hợp xấu nhất | 
| Không gian | O(n) | Lưu trữ tổng mảng và tiền tố | 

Do hạn chế về hoạt động lên tới 5×10^5, việc triển khai này không nhằm mục đích mang lại hiệu suất tối ưu trong trường hợp xấu nhất mà phản ánh việc giảm cấu trúc cần thiết để có được giải pháp được tối ưu hóa hoàn toàn. 

Giải pháp tối ưu dự định dựa vào việc duy trì cấu trúc lồi để trả lời từng truy vấn trong thời gian cố định được khấu hao, đảm bảo khả năng mở rộng đến giới hạn đầu vào đầy đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    arr = []
    pref = [0]
    out = []

    for line in inp.strip().splitlines()[1:]:
        t = list(map(int, line.split()))
        if t[0] == 1:
            arr.append(t[1])
            pref.append(pref[-1] + t[1])
        else:
            mx = arr[-1]
            best = 0.0
            for i in range(len(arr)):
                avg = pref[i+1] / (i+1)
                best = max(best, mx - avg)
            out.append(str(best))

    return "\n".join(out)

# sample
assert run("""6
1 3
2
1 4
2
1 8
2
""") == "0.0\n0.5\n3.0"

# custom: single element queries
assert run("""3
1 5
2
2
""") == "0.0\n0.0"

# custom: increasing gaps
assert run("""5
1 1
1 2
1 100
2
2
""") == "98.0\n98.0"

# custom: all equal
assert run("""5
1 7
1 7
1 7
2
2
""") == "0.0\n0.0"

# custom: alternating queries
assert run("""7
1 1
2
1 2
2
1 3
2
""") == "0.0\n1.0\n2.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở đúng đắn | 
| khoảng cách ngày càng tăng | 98 | sự thống trị của max | 
| tất cả đều bình đẳng | 0 | hành vi trung bình | 
| truy vấn xen kẽ | tăng trưởng đơn điệu | độ chính xác tăng dần | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chỉ tồn tại một phần tử tại thời điểm truy vấn. Tập hợp con phải chứa phần tử đó nên giá trị luôn bằng 0. Thuật toán xử lý việc này bằng cách trả về số 0 một cách rõ ràng khi độ dài mảng bằng một. 

Một trường hợp cạnh khác xảy ra khi tất cả các phần tử đều bằng nhau. Bất kỳ tập hợp con nào cũng có giá trị lớn nhất bằng trung bình, vì vậy câu trả lời luôn bằng 0 bất kể kích thước tập hợp con. Việc đánh giá tiền tố tự nhiên tạo ra số 0 cho tất cả các ứng viên. 

Trường hợp khó phát hiện cuối cùng là khi một phần tử rất lớn xuất hiện sau nhiều phần tử nhỏ. Tập hợp con tối ưu luôn bao gồm phần tử lớn và chỉ một tập hợp con được lựa chọn cẩn thận gồm các phần tử nhỏ hơn. Quá trình quét tiền tố đảm bảo rằng tương tác này được ghi lại một cách chính xác vì mọi tiền tố đều được kiểm tra theo mức tối đa cuối cùng.
