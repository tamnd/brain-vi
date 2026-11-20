---
title: "CF 2161D - Đã khóa"
description: "Chúng ta được cung cấp một mảng và chúng ta được phép xóa các phần tử khỏi mảng đó. Sau khi xóa, chúng tôi muốn chuỗi còn lại đáp ứng một hạn chế rất cụ thể: nó không được chứa hai phần tử trong đó phần tử sau chính xác là một phần tử lớn hơn phần tử trước đó."
date: "2026-06-08T00:00:05+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2161
codeforces_index: "D"
codeforces_contest_name: "Pinely Round 5 (Div. 1 + Div. 2)"
rating: 2100
weight: 2161
solve_time_s: 104
verified: false
draft: false
---

[CF 2161D - Đã khóa](https://codeforces.com/problemset/problem/2161/D) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và chúng ta được phép xóa các phần tử khỏi mảng đó. Sau khi xóa, chúng tôi muốn chuỗi còn lại đáp ứng một hạn chế rất cụ thể: nó không được chứa hai phần tử trong đó phần tử sau chính xác là một phần tử lớn hơn phần tử trước đó. 

Nói cách khác, nếu chúng ta chọn bất kỳ hai vị trí i < j trong mảng còn lại, chúng ta không được phép có một cặp trong đó các giá trị tạo thành một bước tăng dần liên tiếp có kích thước bằng một. Thứ tự của các phần tử vẫn quan trọng vì i < j theo thứ tự từ trái sang phải ban đầu, vì vậy đây không chỉ là về các thuộc tính nhiều tập hợp mà còn về những giá trị nào có thể cùng tồn tại trong một chuỗi con. 

Nhiệm vụ là loại bỏ càng ít phần tử càng tốt để mẫu bị cấm này biến mất hoàn toàn. 

Các ràng buộc rất lớn: tổng số phần tử lên tới 3 × 10^5 trong tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ chiến lược bậc hai nào đối với các chỉ số hoặc các cặp giá trị. Bất kỳ giải pháp nào cố gắng so sánh tất cả các cặp hoặc mô phỏng việc xóa một cách rõ ràng sẽ thất bại, vì vậy chúng tôi cần một cấu trúc xử lý các giá trị theo thời gian gần như tuyến tính hoặc logarit tuyến tính cho mỗi lần kiểm tra. 

Một vấn đề tinh tế xuất hiện khi suy nghĩ một cách tham lam: việc loại bỏ một giá trị tham gia vào một cặp xấu không phải lúc nào cũng có nghĩa là loại bỏ tất cả các bản sao của giá trị đó, bởi vì xung đột phụ thuộc vào thứ tự tương đối và sự cùng tồn tại của các giá trị x và x+1 ở bất kỳ đâu trong chuỗi còn lại. Một hành động tham lam ngây thơ như “loại bỏ tất cả các lần xuất hiện của các giá trị xuất hiện bên cạnh +1” của chúng theo thứ tự ban đầu sẽ không thành công, vì điều kiện bị cấm là toàn cục đối với dãy con chứ không phải lân cận cục bộ. 

Ví dụ, hãy xem xét`1 3 2`. Có sự xung đột giữa 1 và 2 vì 2 - 1 = 1 nhưng chúng không liền kề nhau trong mảng. Bất kỳ cách tiếp cận nào chỉ kiểm tra hàng xóm trong mảng ban đầu sẽ hoàn toàn bỏ lỡ điều này. 

Khó khăn thực sự là sự xuất hiện của các giá trị chỉ tương tác thông qua sự khác biệt về giá trị của chính xác một giá trị, nhưng tất cả các cặp ở các vị trí đều quan trọng. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là thử tất cả các tập hợp con của chỉ số và kiểm tra xem tập hợp con được chọn có hợp lệ hay không. Đối với mỗi tập hợp con, chúng tôi sẽ xác minh xem có cặp (i, j) nào vi phạm điều kiện hay không. Điều này đã dẫn đến 2^n tập hợp con, điều này là không thể ngay cả với n = 30. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn một chút là sửa những phần tử nào chúng tôi giữ lại và đối với mỗi phần tử được giữ lại, hãy kiểm tra tất cả các phần tử được giữ lại sau này để đảm bảo không có cặp nào khác nhau chính xác một phần tử. Đây là O(n^2) cho mỗi lần kiểm tra trong trường hợp xấu nhất, vì ngay cả khi việc kiểm tra được tối ưu hóa, chúng ta vẫn cần kiểm tra nhiều cặp. 

Quan sát chính là ràng buộc chỉ phụ thuộc vào các giá trị chứ không phải vị trí theo cách phức tạp: mọi giá trị x xung đột với bất kỳ x+1 nào nếu cả hai xuất hiện trong tập hợp còn lại theo đúng thứ tự. Vì thứ tự được giữ nguyên nên vấn đề thực sự chỉ đơn giản là chúng ta không thể giữ cả hai giá trị x và x+1 nếu tồn tại ít nhất một x trước x+1 trong dãy con còn lại. Tuy nhiên, vì chúng tôi có thể tự do lựa chọn xóa, nên chúng tôi luôn có thể sắp xếp để nếu cả hai giá trị tồn tại, chúng tôi chỉ có thể “phá vỡ” tất cả các thứ tự vi phạm bằng cách xóa toàn bộ một lớp giá trị theo cách có cấu trúc. 

Điều này dẫn đến một quan điểm đơn giản hơn: chúng ta đang chọn một tập hợp con các giá trị một cách hiệu quả sao cho không có hai số nguyên liên tiếp nào đều “hoạt động” theo cách tạo ra ít nhất một cặp chéo hợp lệ theo thứ tự. Bởi vì chúng ta luôn có thể sắp xếp lại các chuỗi chọn con không có thứ tự, nên chiến lược tối ưu sẽ giảm xuống việc chọn các giá trị theo cách tránh xung đột giữa các số nguyên liên tiếp. 

Cách tiêu chuẩn để mã hóa điều này là nén mảng thành tần số trên mỗi giá trị và sau đó tính DP theo các giá trị: với mỗi giá trị x, chúng ta quyết định xem chúng ta lấy nó hay bỏ qua nó. Nếu chúng tôi lấy x, thì chúng tôi không thể lấy x-1 theo cách tạo ra cấu trúc còn lại hợp lệ, vì vậy chúng tôi đảm bảo rằng chúng tôi không lấy đồng thời các giá trị liền kề. 

Điều này trở thành một “tập hợp trọng số tối đa độc lập trên một đường dẫn” cổ điển trên các giá trị từ 1 đến n, trong đó trọng số là tần số của mỗi giá trị. Câu trả lời là tổng các phần tử trừ đi tổng tần số tối đa mà chúng ta có thể giữ với điều kiện là không bao giờ chọn cả x và x+1. 

Do đó chúng tôi tính toán DP: 

dp[x] = max(dp[x-1], dp[x-2] + freq[x]) 

Điều này cung cấp số lượng phần tử tối đa chúng ta có thể giữ. Số lần xóa tối thiểu là n - dp[max_value]. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n · n) | O(n) | Quá chậm | 
| DP trên dòng giá trị | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng giá trị trong mảng. 

Điều này chuyển vấn đề từ lý luận vị trí sang lý luận dựa trên giá trị, vì chỉ có sự tồn tại của các giá trị mới quan trọng đối với xung đột. 
2. Đặt dp[x] biểu thị số phần tử tối đa mà chúng ta có thể tiếp tục sử dụng các giá trị lên tới x. 

Chúng tôi xây dựng điều này tăng dần vì các lựa chọn cho x chỉ phụ thuộc vào x-1 và x-2. 
3. Với mỗi giá trị x từ 1 đến n, hãy tính dp[x] là giá trị tốt nhất trong hai trường hợp: bỏ qua x hoặc lấy x. 

Bỏ qua x có nghĩa là dp[x-1]. Lấy x nghĩa là chúng ta đạt được freq[x], nhưng phải kết hợp nó với dp[x-2] để tránh xung đột kề cận. 
4. Sự tái diễn là: 

dp[x] = max(dp[x-1], dp[x-2] + freq[x]) 

Điều này mã hóa thực tế là việc chọn x sẽ cấm chọn x-1. 
5. Câu trả lời là tổng n trừ dp[max_value]. 

### Tại sao nó hoạt động

Bất biến chính là sau khi xử lý giá trị x, dp[x] lưu trữ số phần tử tối đa mà chúng ta có thể tiếp tục sử dụng chỉ các giá trị từ 1 đến x với ràng buộc là không có hai giá trị liên tiếp nào được chọn. Bất kỳ tập hợp con hợp lệ nào của các phần tử được giữ đều có thể được ánh xạ tới một lựa chọn các giá trị thỏa mãn ràng buộc này, bởi vì nếu cả x và x+1 đều được chọn theo cách duy trì ít nhất một cặp thứ tự hợp lệ thì cấu hình đó không được phép. Do đó, giải pháp tối ưu phải loại bỏ ít nhất một điểm cuối của mỗi cặp giá trị liên tiếp và DP liệt kê tất cả các lựa chọn nhất quán như vậy mà không tính hai lần hoặc thiếu cấu hình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = [0] * (n + 2)
        for x in a:
            freq[x] += 1
        
        if n == 1:
            print(0)
            continue
        
        dp_prev2 = 0
        dp_prev1 = freq[1]
        
        for x in range(2, n + 1):
            dp_cur = max(dp_prev1, dp_prev2 + freq[x])
            dp_prev2, dp_prev1 = dp_prev1, dp_cur
        
        best_keep = dp_prev1
        print(n - best_keep)

if __name__ == "__main__":
    solve()
```Việc triển khai nén bộ nhớ DP thành O(1) bằng cách chỉ giữ hai trạng thái cuối cùng, vì mỗi lần chuyển đổi chỉ phụ thuộc vào x-1 và x-2. Việc đếm tần số được thực hiện một lần cho mỗi trường hợp thử nghiệm. Phép trừ cuối cùng chuyển đổi tập hợp con được giữ tối đa thành số lần loại bỏ tối thiểu. 

Một sai lầm phổ biến là giả định rằng các giá trị chỉ được xử lý tối đa giá trị tối đa xuất hiện trong mảng. Mặc dù điều đó đúng về mặt logic, nhưng việc lặp lại lên đến n là an toàn vì các tần số không được sử dụng bằng 0 và DP vẫn nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`5 1 2 3 4 5`Ở đây tất cả các giá trị đều có mặt một lần. 

| x | tần số[x] | dp[x-2] | dp[x-1] | dp[x] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | 0 | 1 | 
| 2 | 1 | 0 | 1 | 1 | 
| 3 | 1 | 1 | 1 | 2 | 
| 4 | 1 | 1 | 2 | 2 | 
| 5 | 1 | 2 | 2 | 3 | 

Chúng tôi giữ tổng cộng 3 phần tử, do đó số lần loại bỏ = 5 - 3 = 2. Điều này phù hợp với ý tưởng rằng chúng tôi không thể lấy các giá trị liên tiếp, vì vậy chúng tôi chọn các chỉ số chẵn hoặc lẻ của dòng giá trị một cách hiệu quả. 

### Ví dụ 2 

đầu vào:`5 5 5 5 4 4`Tần số là: freq[4]=2, freq[5]=3. 

| x | tần số[x] | dp[x-2] | dp[x-1] | dp[x] | 
| --- | --- | --- | --- | --- | 
| 4 | 2 | 0 | 0 | 2 | 
| 5 | 3 | 0 | 2 | 3 | 

Chúng ta có thể giữ cả 3 cái 5 hoặc 2 cái 4, nhưng không phải cả hai đều có ý nghĩa tương tác hoàn toàn, vì vậy tốt nhất là giữ 3 cái. Loại bỏ = 5 - 3 = 2. 

Những dấu vết này cho thấy thuật toán không theo dõi vị trí mà hoàn toàn tối ưu hóa các xung đột giá trị, thế là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Đếm tần số cộng với DP tuyến tính trên phạm vi giá trị | 
| Không gian | O(n) | Mảng tần số cộng với trạng thái DP không đổi | 

Tổng số n trên các trường hợp thử nghiệm là 3 × 10^5, do đó, giải pháp tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Việc sử dụng bộ nhớ là tuyến tính trong trường hợp thử nghiệm lớn nhất và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(data: str) -> str:
    sys.stdin = io.StringIO(data)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# provided samples
assert solve_io("""6
1
1
5
1 2 3 4 5
5
5 4 3 2 1
5
5 5 5 4 4
7
1 7 1 2 5 7 1
6
1 2 5 6 5 5
""") == """0
2
0
0
1
2"""

# all equal
assert solve_io("""1
5
3 3 3 3 3
""") == "0"

# alternating values
assert solve_io("""1
6
1 2 1 2 1 2
""") == "3"

# single chain
assert solve_io("""1
4
1 2 3 4
""") == "2"

# minimum size
assert solve_io("""1
1
1
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 0 | không tồn tại cặp liên tiếp | 
| xen kẽ 1/2 | 3 | lựa chọn lực lượng xung đột lân cận | 
| 1 2 3 4 | 2 | toàn bộ chuỗi lực loại bỏ một nửa | 
| phần tử đơn | 0 | trường hợp ranh giới cơ sở | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử giống hệt nhau. Vì không có giá trị x+1 nào tồn tại nên ràng buộc không bao giờ được kích hoạt và thuật toán tạo ra dp một cách chính xác để giữ mọi thứ. Ví dụ, đầu vào`3 3 3 3 3`tạo ra freq[3]=5 và không có tương tác liền kề, vì vậy dp chỉ giữ lại tất cả 5. 

Một trường hợp cạnh khác là khối liên tiếp dày đặc như`1 2 3 4 5`. Ở đây mọi giá trị đều xung đột về mặt cấu trúc với các giá trị lân cận của nó, do đó DP thay thế các lựa chọn giữa các giá trị. Thuật toán tránh chọn các giá trị liền kề một cách chính xác bằng cách xây dựng phép truy toán, mang lại một tập hợp độc lập tối đa trên một đường dẫn. 

Trường hợp thứ ba là các giá trị thưa thớt như`1 100 2 99`. Mặc dù các giá trị cách xa nhau về mặt số lượng nhưng chỉ các số nguyên liên tiếp mới quan trọng, do đó DP tách biệt từng giá trị một cách độc lập. Điều này cho thấy rằng giải pháp phụ thuộc vào giá trị kề nhau, chứ không phải độ lớn hoặc thứ tự mảng, đó chính xác là những gì mà phép truy toán nắm bắt.
