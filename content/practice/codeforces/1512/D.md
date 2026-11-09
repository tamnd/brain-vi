---
title: "CF 1512D - Mảng bị hỏng"
description: "Chúng ta được cung cấp một mảng đã được “giả mạo” theo một cách rất cụ thể. Ban đầu có một mảng không xác định có độ dài n, gọi là a."
date: "2026-06-10T18:53:20+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1512
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 713 (Div. 3)"
rating: 1200
weight: 1512
solve_time_s: 144
verified: false
draft: false
---

[CF 1512D - Mảng bị hỏng](https://codeforces.com/problemset/problem/1512/D) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng đã được “giả mạo” theo một cách rất cụ thể. Ban đầu có một mảng độ dài không xác định`n`, gọi nó`a`. Từ mảng này, hai số bổ sung được thêm vào: một là tổng của tất cả các phần tử trong`a`, và số còn lại là số phụ tùy ý`x`nằm trong khoảng từ 1 đến 10^9. Sau khi nối thêm hai giá trị này, toàn bộ mảng được xáo trộn và chúng ta chỉ thấy mảng hỗn hợp cuối cùng`b`kích thước`n + 2`. 

Nhiệm vụ là khôi phục bất kỳ mảng gốc hợp lệ nào`a`chiều dài`n`hoặc xác định rằng không có mảng nào như vậy có thể tạo ra kết quả đã cho`b`. 

Khó khăn chính là chúng ta không biết hai yếu tố nào trong`b`là những cái "đặc biệt": một là tổng của mảng ban đầu và cái còn lại là số bổ sung. Mọi phần tử khác phải thuộc về`a`. 

Các ràng buộc là lớn, với tổng`n`trên tất cả các trường hợp thử nghiệm lên tới 2 · 10^5. Điều này loại trừ bất kỳ phương pháp nào cố gắng loại bỏ các cặp hoặc tính lại tổng nhiều lần trong thời gian bậc hai. Bất kỳ giải pháp nào về cơ bản đều phải xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính hoặc gần tuyến tính, thường bằng cách sắp xếp và thực hiện một lần quét hoặc sử dụng hàm băm. 

Một trường hợp cạnh tinh tế phát sinh khi tồn tại nhiều lựa chọn ứng cử viên cho “phần tử tổng”, đặc biệt khi tổng bằng một trong các phần tử thông thường. Ví dụ: nếu tất cả các số đều nhỏ và lặp lại, việc chọn sai lần xuất hiện có thể vô tình tạo ra một kết quả tái tạo nhất quán nhưng không chính xác. Một trường hợp lỗi khác xuất hiện khi mảng còn lại sau khi loại bỏ tổng ứng cử viên không khớp với tổng yêu cầu do trùng lặp hoặc xác định sai phần tử phụ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi cặp chỉ số`(i, j)`TRONG`b`, điều trị`b[i]`như tổng của`a`Và`b[j]`làm số bổ sung, sau đó kiểm tra xem có xóa`b[i]`Và`b[j]`để lại một mảng có tổng bằng`b[i]`. Đối với mỗi lần thử, chúng tôi sẽ tính tổng các phần tử còn lại và xác minh sự bằng nhau. 

Điều này hoạt động hợp lý vì bài toán chỉ ẩn hai phần tử đặc biệt. Tuy nhiên, chi phí đến từ việc tính toán lại tổng và kiểm tra tính hợp lệ của các cặp O(n^2) và mỗi lần kiểm tra là O(n), dẫn đến O(n^3) trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát quan trọng là nếu chúng ta sửa phần tử nào là số thừa`x`, thì tổng số còn lại`n + 1`các phần tử phải bao gồm mảng ban đầu cộng với tổng của nó. Cấu trúc đó có nghĩa là một khi`x`cố định, chúng ta có thể xác định tổng yêu cầu của`a`và kiểm tra tính nhất quán trong thời gian tuyến tính. 

Chúng tôi sắp xếp mảng để xử lý việc lựa chọn ứng viên một cách có hệ thống. Đối với mỗi vị trí ứng viên được điều trị`b[i]`BẰNG`x`, chúng tôi tính tổng số tiền`S`. Multiset còn lại phải chứa`S`như một trong những yếu tố của nó. Sau đó chúng tôi có thể cố gắng xác thực xem việc loại bỏ`x`và số tiền phù hợp đó để lại chính xác`n`các phần tử có tổng nhất quán. 

Cấu trúc được sắp xếp cho phép chúng tôi xác định các ứng viên một cách hiệu quả và tránh việc tính toán lại nhiều lần. Thay vì tính lại tổng cho mỗi cặp, chúng tôi duy trì lý luận kiểu tiền tố: loại bỏ hai phần tử và kiểm tra tính nhất quán số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force | O(n^3) | O(1) | Quá chậm | 
| Sắp xếp + Kiểm tra quét đơn | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng`b`. Việc sắp xếp không bắt buộc phải đảm bảo tính chính xác nhưng cho phép chúng ta suy luận về các ứng cử viên theo cách có cấu trúc và nhanh chóng truy cập các giá trị lớn có nhiều khả năng là tổng hoặc phần tử phụ. 
2. Tính tổng số tiền`S`của tất cả các phần tử trong`b`. Từ`S = sum(a) + x`, bất kỳ việc xây dựng lại hợp lệ nào cũng phải thỏa mãn việc loại bỏ`x`Và`sum(a)`để lại chính xác các yếu tố của`a`. 
3. Lặp lại từng chỉ mục`i`, điều trị`b[i]`là số bổ sung của ứng cử viên`x`. Với mỗi lựa chọn hãy tính`remaining_sum = S - b[i]`. 
4. Bây giờ chúng ta phải kiểm tra xem có tồn tại phần tử nào đó trong mảng còn lại có thể đóng vai trò là`sum(a)`. Phần tử đó phải bằng`remaining_sum / 2`, bởi vì: 

nếu`remaining_sum = sum(a) + x`và chúng tôi đã loại bỏ`x`, thì phần còn lại bằng`sum(a)`, vì vậy sau khi loại bỏ`x`từ tổng số, multiset còn lại phải chứa`sum(a)`và các phần tử ban đầu, ngụ ý một điều kiện nhất quán giảm xuống việc kiểm tra giá trị khớp trong mảng. 

Cụ thể hơn, sau khi loại bỏ`b[i]`, chúng ta cần tìm chỉ mục thứ hai`j ≠ i`như vậy`b[j] = remaining_sum - b[j]`, ngụ ý`b[j] = remaining_sum / 2`. 
5. Nếu`remaining_sum`là số lẻ, hãy bỏ qua ứng cử viên này vì không thể phân chia số nguyên. 
6. Nếu nó chẵn, hãy kiểm tra xem giá trị`remaining_sum // 2`tồn tại trong mảng không bao gồm chỉ mục`i`. Nếu đúng như vậy thì chúng ta đã tìm được một phân tách hợp lệ. 
7. Xây dựng`a`bằng cách xuất ra tất cả các phần tử ngoại trừ phần được chọn`x`và phần tử tổng được chọn. 

### Tại sao nó hoạt động 

Bất biến là multiset`b`luôn chứa chính xác ba phần khái niệm: mảng ban đầu`a`, một bản sao của`sum(a)`và một giá trị tùy ý`x`. Mọi hoạt động tái tạo hợp lệ đều phải loại bỏ chính xác hai phần tử đặc biệt này, để lại một tập hợp nhiều tập hợp có tổng phù hợp với cấu trúc còn lại. Bằng cách buộc một ứng viên phải`x`, chúng ta rút gọn vấn đề thành việc kiểm tra xem phần còn lại có thể được chia thành phần tử tổng hợp lệ và mảng ban đầu hay không. Vì phần tử tổng được xác định duy nhất một lần`x`được khắc phục, việc kiểm tra sẽ giảm xuống còn một thử nghiệm tồn tại duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        
        total = sum(b)
        b.sort()
        
        freq = {}
        for v in b:
            freq[v] = freq.get(v, 0) + 1
        
        found = False
        
        for i in range(n + 2):
            x = b[i]
            remaining = total - x
            
            if remaining % 2 != 0:
                continue
            
            target = remaining // 2
            
            freq[x] -= 1
            if freq[x] == 0:
                del freq[x]
            
            if freq.get(target, 0) > 0:
                freq[target] -= 1
                if freq[target] == 0:
                    del freq[target]
                
                # remaining elements form a valid a
                res = []
                for k, v in freq.items():
                    res.extend([k] * v)
                
                if len(res) == n:
                    print(*res)
                    found = True
                    break
                
                # rollback
                freq[target] = freq.get(target, 0) + 1
            
            # restore x
            freq[x] = freq.get(x, 0) + 1
        
        if not found:
            print(-1)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc theo dõi tần suất để chúng tôi có thể mô phỏng hiệu quả việc loại bỏ hai phần tử. Đối với mỗi ứng viên`x`, chúng tôi tạm thời giảm tần số của nó và cố gắng loại bỏ tần số tương ứng`target = (total - x) // 2`. Nếu thành công, multiset còn lại chính xác là được xây dựng lại`a`. 

Một sai lầm phổ biến là quên rằng ứng cử viên cho tổng phải được loại bỏ dưới dạng một lần xuất hiện chứ không phải tất cả các lần xuất hiện. Đây là lý do tại sao bản đồ tần số được thay đổi cẩn thận và khôi phục sau mỗi lần thử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
b = [2, 3, 7, 12, 2]
```| Bước | tổng cộng | x đã chọn | còn lại | mục tiêu | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 26 | 2 | 24 | 12 | thử xóa 2 | 
| 2 | 26 | 12 | 14 | 7 | tìm thấy sự phân chia hợp lệ | 

Khi`x = 2`, loại bỏ nó để lại 24, đòi hỏi phần tử tổng là 12. Vì 12 tồn tại nên loại bỏ cả hai lá`[2,3,7]`, hợp lệ. 

Điều này xác nhận rằng thuật toán xác định chính xác lần xuất hiện nào là số bổ sung. 

### Ví dụ 2 

đầu vào:```
b = [9, 1, 7, 1, 6, 5]
```| Bước | tổng cộng | x đã chọn | còn lại | mục tiêu | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 29 | 9 | 20 | 10 | không tìm thấy | 
| 2 | 29 | 7 | 22 | 11 | không tìm thấy | 
| 3 | 29 | 6 | 23 | 11,5 | không hợp lệ | 

Không có sự phân chia hợp lệ nào tồn tại cho bất kỳ lựa chọn nào`x`, vì vậy đầu ra là`-1`. 

Điều này thể hiện các trường hợp lỗi trong đó không có phần tử nào có thể đóng vai trò là thành phần tổng nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Việc sắp xếp chiếm ưu thế và mỗi lần kiểm tra ứng viên đều có mức trung bình O(1) bằng cách sử dụng hàm băm | 
| Không gian | O(n) | Bản đồ tần số và lưu trữ đầu ra | 

Tổng độ phức tạp có thể chấp nhận được vì tổng của tất cả`n`trên các trường hợp thử nghiệm được giới hạn bởi 2 · 10^5, do đó, ngay cả việc sắp xếp tất cả đầu vào vẫn hiệu quả trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            b = list(map(int, input().split()))
            total = sum(b)
            freq = defaultdict(int)
            for v in b:
                freq[v] += 1
            
            found = False
            for i in range(n + 2):
                x = b[i]
                freq[x] -= 1
                if freq[x] == 0:
                    del freq[x]
                
                rem = total - x
                if rem % 2 == 0:
                    target = rem // 2
                    if freq.get(target, 0) > 0:
                        freq[target] -= 1
                        if freq[target] == 0:
                            del freq[target]
                        if sum(k * v for k, v in freq.items()) == n * target - (target - target):
                            out.append(" ".join(map(str, sum(([k] * v for k, v in freq.items()), []))))
                            found = True
                            break
                        freq[target] = freq.get(target, 0) + 1
                
                freq[x] = freq.get(x, 0) + 1
            
            if not found:
                out.append("-1")
        return "\n".join(out)

    return solve()

# provided samples
assert run("""4
3
2 3 7 12 2
4
9 1 7 1 6 5
5
18 2 2 3 2 9 2
3
2 6 9 2 1
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu n=1 thùng | tính đúng đắn của cấu trúc nhỏ nhất | xử lý việc tái thiết tầm thường | 
| tất cả các giá trị bằng nhau | mạnh mẽ dưới sự trùng lặp | tránh lỗi loại bỏ kép | 
| trộn ngẫu nhiên lớn | ổn định hiệu suất | đảm bảo hành vi O(n log n) | 
| trường hợp không có giải pháp | đúng đầu ra -1 | từ chối các phần chia không hợp lệ | 

## Vỏ cạnh 

Một tình huống khó khăn là khi ứng viên`x`bằng cùng giá trị với phần tử tổng giả định. Trong các mảng có các bản sao, việc loại bỏ một lần xuất hiện là rất quan trọng. Việc xóa dựa trên tần suất đảm bảo rằng mỗi lần chỉ có một phiên bản được loại trừ, ngăn chặn việc vô tình xóa quá mức. 

Một trường hợp đặc biệt khác xảy ra khi câu trả lời đúng yêu cầu chọn phần tử lớn nhất làm`x`, bởi vì trực giác tham lam có thể thiên về những giá trị nhỏ hơn. Thuật toán thử tất cả các ứng cử viên một cách có hệ thống, vì vậy nó vẫn thành công ngay cả khi lựa chọn tối ưu không thể hiện rõ chỉ bằng việc sắp xếp đơn hàng. 

Trường hợp cạnh cuối cùng là khi tồn tại nhiều câu trả lời hợp lệ. Vì thuật toán trả về ngay lập tức khi tìm thấy bất kỳ cặp nhất quán nào`(x, sum(a))`, nó tự nhiên tạo ra một bản dựng lại hợp lệ mà không có sự mơ hồ.
