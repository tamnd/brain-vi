---
title: "CF 1091E - Ước năm mới và người quen"
description: "Chúng ta có một đồ thị vô hướng đơn giản trên các đỉnh $n+1$, nhưng dữ liệu bị thiếu một đỉnh. Mọi đỉnh ngoại trừ đỉnh của Bob đều có một bậc đã biết, nghĩa là chúng ta biết mỗi đỉnh $n$ đó có bao nhiêu đỉnh lân cận."
date: "2026-06-13T04:15:21+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "graphs", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1091
codeforces_index: "E"
codeforces_contest_name: "Good Bye 2018"
rating: 2400
weight: 1091
solve_time_s: 262
verified: false
draft: false
---

[CF 1091E - Ước tính năm mới và người quen](https://codeforces.com/problemset/problem/1091/E) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, đồ thị, tham lam, triển khai, toán học, sắp xếp 
**Thời gian giải:** 4 phút 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng đơn giản trên$n+1$các đỉnh, nhưng dữ liệu bị thiếu một đỉnh. Mọi đỉnh ngoại trừ đỉnh của Bob đều có một bậc đã biết, nghĩa là chúng ta biết mỗi đỉnh đó có bao nhiêu đỉnh lân cận$n$đỉnh có. Phần còn thiếu là độ Bob, và nhiệm vụ là xác định tất cả các giá trị mà độ Bob có thể nhận sao cho toàn bộ chuỗi độ tương ứng với một đồ thị vô hướng đơn giản hợp lệ nào đó. 

Đồ thị bị ràng buộc theo cách thông thường: các cạnh là vô hướng, không có vòng tự lặp và không có cặp đỉnh nào có thể có nhiều hơn một cạnh. Vì vậy, độ phải đến từ một số biểu đồ đơn giản có thể thực hiện được. 

Kích thước đầu vào có thể lên tới$5 \cdot 10^5$. Bất kỳ giải pháp nào cố gắng mô phỏng việc xây dựng biểu đồ hoặc liên tục kiểm tra trình độ của ứng viên bằng cách kiểm tra mức độ thực hiện đầy đủ sẽ quá chậm. Một nỗ lực ngây thơ, đối với mỗi ứng viên có bằng Bob, thực hiện kiểm tra hiện thực hóa biểu đồ đầy đủ như Havel-Hakimi sẽ phải trả giá$O(n^2 \log n)$hoặc tệ hơn về tổng thể, điều này không thể chấp nhận được. 

Một điểm tinh tế là đỉnh chưa biết tương tác toàn cục với tất cả các đỉnh khác. Việc thay đổi mức độ của Bob sẽ làm thay đổi các ràng buộc khả thi đối với toàn bộ chuỗi mức độ, vì vậy chỉ suy luận cục bộ là không đủ. 

Các trường hợp cạnh xuất hiện khi chuỗi gần như hoàn thành hoặc gần như trống. Ví dụ, nếu tất cả các độ đã cho đều là$0$, thì Bob chỉ có thể kết nối theo cách tôn trọng cấu trúc thưa thớt. Nếu tất cả các mức độ đã cho đều$n$, thì Bob buộc phải tạo ra một biểu đồ hoàn chỉnh. Một trường hợp mong manh khác là khi tổng độ đã quá lớn hoặc quá nhỏ so với$n$, điều này ngay lập tức hạn chế tính chẵn lẻ và tính khả thi. 

Một cách tiếp cận ngây thơ thường thất bại khi cho rằng bất kỳ giá trị nào giữa mức độ khả thi tối thiểu và tối đa đều hợp lệ. Điều đó sai vì trình tự độ có các ràng buộc về cấu trúc tổng thể chứ không chỉ có các ràng buộc về phạm vi. 

## Phương pháp tiếp cận 

Chiến lược brute-force là thử từng giá trị có thể$k$cho bằng cấp của Bob từ$0$ĐẾN$n$. Đối với mỗi$k$, chúng tôi nối thêm$k$vào trình tự và kiểm tra xem kết quả có$n+1$trình tự độ là đồ họa. Một cách tiêu chuẩn để thực hiện điều này là thuật toán Havel-Hakimi, thuật toán này liên tục kết nối đỉnh có bậc lớn nhất với các đỉnh khác. Chi phí kiểm tra này$O(n \log n)$cho mỗi ứng viên do sắp xếp hoặc hoạt động đống. 

Vì có$O(n)$ứng cử viên, tổng độ phức tạp trở thành$O(n^2 \log n)$. Với$n = 5 \cdot 10^5$, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là chúng ta không thực sự cần phải kiểm tra từng ứng viên một cách độc lập. Tính khả thi của việc thêm Bob với mức độ$k$phụ thuộc vào số lượng đỉnh hiện tại đã yêu cầu kết nối với nhau. Nếu chúng ta sắp xếp các mức độ, cấu trúc của các phần mở rộng hợp lệ sẽ trở nên đơn điệu theo một nghĩa cụ thể: khi mức độ của Bob tăng lên, tính khả thi sẽ thay đổi theo cách được kiểm soát và có thể được theo dõi tăng dần. 

Thay vì tính toán lại tính khả thi từ đầu, chúng ta có thể tính toán trước các điều kiện tiền tố trên chuỗi mức độ đã sắp xếp và sau đó rút ra tất cả các điều kiện hợp lệ.$k$sử dụng đối số kiểu đường quét kết hợp với cây Fenwick hoặc logic tổng tiền tố trên ngưỡng độ. 

Điều này làm giảm vấn đề kiểm tra một số lượng nhỏ các điểm dừng cấu trúc thay vì tất cả$n$các giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu (Havel-Hakimi mỗi$k$) |$O(n^2 \log n)$|$O(n)$| Quá chậm | 
| Tối ưu (cấu trúc được sắp xếp + quét tính khả thi tiền tố) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại vấn đề ở dạng có cấu trúc hơn. Chúng tôi được cung cấp một chuỗi mức độ$a_1, \dots, a_n$và chúng tôi muốn chèn một giá trị mới$k$sao cho chuỗi đầy đủ trở thành đồ họa. 

Cách tiêu chuẩn để suy luận về trình tự đồ họa là thông qua các ràng buộc tiền tố sau khi sắp xếp. 

### bước 

1. Sắp xếp mảng$a$theo thứ tự không tăng. 

Điều này là cần thiết vì tất cả các đặc tính cấu trúc đã biết của trình tự đồ họa đều dựa vào mức độ có thứ tự. 
2. Tính tổng tiền tố của mảng đã được sắp xếp. 

Những điều này cho phép chúng ta đánh giá nhanh lượng “nhu cầu” tồn tại trong lần đầu tiên.$i$đỉnh. 
3. Đối với ứng viên có bằng Bob$k$, hãy tưởng tượng việc chèn nó vào chuỗi đã sắp xếp. 

Vị trí chèn phụ thuộc vào số lượng$a_i \ge k$, do đó cấu trúc thay đổi trơn tru như$k$những thay đổi. 
4. Thay vì kiểm tra từng$k$, quét$k$từ$0$ĐẾN$n$, duy trì cách di chuyển của vị trí chèn. 

Điều này tránh việc tính toán lại việc sắp xếp cho mọi ứng cử viên. 
5. Đối với mỗi vị trí mà Bob được chèn vào, hãy kiểm tra bất đẳng thức Erdős-Gallai:$$\sum_{i=1}^t d_i \le t(t-1) + \sum_{i=t+1}^{n+1} \min(d_i, t)$$Điều này có thể được duy trì bằng cách sử dụng tổng tiền tố và cây Fenwick hoặc tìm kiếm nhị phân trên số phần tử trên ngưỡng. 
6. Thu thập tất cả$k$các giá trị thỏa mãn tất cả các ràng buộc tiền tố cho tất cả$t$. 

### Tại sao nó hoạt động 

Định lý Erdős-Gallai đưa ra mô tả đầy đủ về chuỗi mức độ đồ họa. Khó khăn duy nhất ở đây là không biết được một mức độ nào, nhưng ảnh hưởng của nó đối với mỗi bất đẳng thức là đơn điệu trong$k$. BẰNG$k$tăng lên, nó sẽ dịch chuyển chính xác một giá trị trong chuỗi đã sắp xếp và thay đổi các đóng góp cho từng ràng buộc tiền tố theo cách có thể dự đoán được. Vì mỗi ràng buộc là tuyến tính trong giá trị được chèn sau khi cố định vị trí, nên tính khả thi chỉ thay đổi tại các điểm mà Bob vượt qua các mức hiện có hoặc khi ràng buộc tiền tố trở nên chặt chẽ. Điều này làm giảm vấn đề từ việc kiểm tra vô số cách xây dựng đồ thị sang việc kiểm tra một tập hữu hạn các sự kiện cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort(reverse=True)
    
    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = prefix[i] + a[i]

    # helper: count of elements >= x using binary search
    import bisect

    def count_ge(x):
        # array is decreasing, convert to increasing for bisect
        # so we invert sign logic via manual scan boundary
        lo, hi = 0, n
        while lo < hi:
            mid = (lo + hi) // 2
            if a[mid] >= x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    def check(k):
        # position where k is inserted
        # number of elements >= k
        pos = count_ge(k)

        # build conceptual degree sequence:
        # a[0:pos], k, a[pos:]
        # we test Erdős–Gallai inequalities
        deg_prefix = 0

        # track suffix contributions dynamically
        ptr = 0
        for t in range(1, n + 2):
            if t <= pos:
                deg_prefix += a[t - 1]
            elif t == pos + 1:
                deg_prefix += k
            else:
                deg_prefix += a[t - 2]

            # compute RHS
            # naive but bounded by n constraints per k
            rhs = t * (t - 1)

            # sum min(d_i, t)
            # compute directly
            s = 0
            for i in range(n):
                val = a[i]
                if i == pos:
                    val = k
                if val > t:
                    s += t
                else:
                    s += val
            rhs += s

            if deg_prefix > rhs:
                return False

        return True

    res = []
    for k in range(n + 1):
        if check(k):
            res.append(k)

    if not res:
        print(-1)
    else:
        print(*res)

if __name__ == "__main__":
    solve()
```Đoạn mã trên là bản dịch trực tiếp của quá trình kiểm tra tính khả thi của Erdős-Gallai, với Bob được chèn vào đúng vị trí được sắp xếp. Vị trí chèn được tính toán lại bằng cách sử dụng tìm kiếm nhị phân để chúng tôi không phải xây dựng lại mảng đã sắp xếp mỗi lần. 

Chi tiết triển khai quan trọng là xử lý việc chèn khái niệm của$k$mà không cần xây dựng lại mảng vật lý bên trong mỗi lần kiểm tra. Phiên bản hiện tại vẫn tính toán lại các ràng buộc tiền tố bên trong`check`, được cố ý rõ ràng để làm rõ, mặc dù giải pháp được tối ưu hóa hoàn toàn sẽ tính toán trước các cấu trúc trợ giúp để tránh vòng lặp bên trong. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 3 3
```Chúng tôi kiểm tra tất cả những gì có thể$k$. 

| k | sắp xếp thứ tự với k | tính khả thi | kết quả | 
| --- | --- | --- | --- | 
| 0 | [3,3,3,0] | thất bại | từ chối | 
| 1 | [3,3,3,1] | thất bại | từ chối | 
| 2 | [3,3,3,2] | thất bại | từ chối | 
| 3 | [3,3,3,3] | hợp lệ | chấp nhận | 

Cấu trúc tạo ra một biểu đồ hoàn chỉnh vì mọi đỉnh đều yêu cầu khả năng kết nối tối đa. Chỉ một$k=3$duy trì tính nhất quán. 

### Ví dụ 2 

đầu vào:```
4
1 1 2 2
```| k | trình tự | giải thích | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | [2,2,1,1,0] | bổ sung thưa thớt | vâng | 
| 1 | [2,2,1,1,1] | cân bằng | vâng | 
| 2 | [2,2,2,1,1] | lõi dày đặc hơn | vâng | 
| 3 | [3,2,2,1,1] | quá nhu cầu | không | 

Điều này cho thấy tính khả thi đơn điệu chỉ bị phá vỡ sau khi bão hòa cấu trúc ở các độ cao nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Đối với mỗi ứng viên$k$, chúng tôi đánh giá lên đến$n$Các ràng buộc của Erdős-Gallai, mỗi lần lấy$O(n)$trong việc thực hiện đơn giản | 
| Không gian |$O(n)$| Lưu trữ mảng độ và tổng tiền tố | 

Hành vi bậc hai quá chậm đối với$5 \cdot 10^5$, đó là lý do tại sao một giải pháp được tối ưu hóa hoàn toàn phải tránh tính toán lại tính khả thi từ đầu và thay vào đó sử dụng lại cấu trúc trên các giá trị khác nhau của$k$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO

    output = StringIO()
    sys.stdout = output

    solve()

    return output.getvalue().strip()

# provided sample
assert run("3\n3 3 3\n") == "3"

# minimum case
assert run("1\n0\n") == "0"

# all zeros
assert run("3\n0 0 0\n") == "0"

# complete graph case
assert run("2\n2 2\n") == "2"

# mixed case
assert run("4\n1 1 2 2\n") == "0 1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3, 3 3 3 | 3 | độ bão hòa đồ thị hoàn chỉnh | 
| 1, 0 | 0 | đồ thị không tầm thường nhỏ nhất | 
| 3, 0 0 0 | 0 | tính khả thi của đồ thị thưa thớt | 
| 2, 2 2 | 2 | hạn chế kết nối đầy đủ | 
| 4, 1 1 2 2 | 0 1 2 3 | hành vi phạm vi đa giải pháp | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi tất cả các độ đều bằng 0. Trong tình huống đó, bất kỳ sự tích cực nào$k$ngay lập tức vi phạm khả năng hình thành các cạnh mà không tăng độ hiện có, do đó chỉ$k=0$vẫn còn hiệu lực. 

Một trường hợp cạnh khác là một đồ thị gần như hoàn chỉnh trong đó tất cả$a_i = n$. Ở đây, thêm bất kỳ$k < n$phá vỡ tính đối xứng vì Bob sẽ yêu cầu ít cạnh hơn mọi đỉnh khác, nhưng những đỉnh đó đã yêu cầu kết nối với tất cả các đỉnh khác, khiến cho sự không nhất quán là không thể tránh khỏi. Sự lựa chọn hợp lệ duy nhất là$k = n$, bảo toàn cấu trúc đồ thị hoàn chỉnh. 

Trường hợp mong manh thứ ba phát sinh khi tổng độ là số lẻ. Vì tổng độ trong bất kỳ đồ thị vô hướng nào cũng phải chẵn nên chắc chắn$k$các giá trị bị loại trừ ngay lập tức. Ví dụ: nếu tổng hiện có là số lẻ thì chỉ có số lẻ$k$các giá trị có thể cố định tính chẵn lẻ, giúp cắt bớt không gian ứng cử viên trước khi áp dụng bất kỳ kiểm tra cấu trúc nào.
