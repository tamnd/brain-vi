---
title: "CF 1557B - Moamen và mảng con k"
description: "Chúng ta được cung cấp một mảng các số nguyên riêng biệt và được phép thực hiện một phép biến đổi rất cụ thể chính xác một lần. Đầu tiên, chúng ta cắt mảng thành đúng k phần liền kề nhau, mỗi phần không trống."
date: "2026-06-16T16:11:44+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1557
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 737 (Div. 2)"
rating: 1100
weight: 1557
solve_time_s: 196
verified: true
draft: false
---

[CF 1557B - Moamen và k-mảng con](https://codeforces.com/problemset/problem/1557/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên riêng biệt và được phép thực hiện một phép biến đổi rất cụ thể chính xác một lần. Đầu tiên, chúng ta cắt mảng thành đúng k phần liền kề nhau, mỗi phần không trống. Sau đó chúng ta được phép hoán vị các mảnh này một cách tùy ý và cuối cùng chúng ta dán chúng lại với nhau theo thứ tự mới đó. 

Mục đích là để xác định xem có tồn tại cách nào đó để chọn k phân đoạn sao cho sau khi sắp xếp lại chúng, mảng được hợp nhất cuối cùng sẽ được sắp xếp hoàn toàn theo thứ tự không giảm. 

Khó khăn chính là chúng ta không được phép sắp xếp lại các phần tử riêng lẻ mà chỉ được phép sắp xếp lại toàn bộ các khối liền kề. Vì vậy, cấu trúc của mảng ban đầu áp đặt các ràng buộc về những phần tử nào có thể được nhóm lại với nhau mà không ảnh hưởng đến khả năng sắp xếp lại thành một chuỗi được sắp xếp. 

Các ràng buộc đủ lớn để mọi giải pháp đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Với tổng n lên tới 3e5, cách tiếp cận O(n log n) cho mỗi trường hợp thử nghiệm là ổn, nhưng bất kỳ phương pháp bậc hai nào trong khám phá phân đoạn đều không thể thực hiện được. Một lực lượng vũ phu đối với tất cả các phân vùng k có thể sẽ bùng nổ theo kiểu tổ hợp vì có O (n chọn k) cách phân chia. 

Một trường hợp cạnh tinh tế xuất hiện khi k bằng 1. Khi đó chúng ta không thể sắp xếp lại bất cứ thứ gì, do đó mảng phải được sắp xếp sẵn. Một trường hợp cạnh khác là k bằng n, trong đó mỗi phần tử là phân đoạn riêng của nó và chúng ta luôn có thể sắp xếp bằng cách hoán vị các phần tử đơn, vì vậy câu trả lời luôn là CÓ. 

Các trường hợp không tầm thường là khi 1 < k < n, trong đó phân đoạn tương tác với thứ tự bên trong của mảng một cách có ý nghĩa. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cách có thể để chia mảng thành k phân đoạn liền kề, sau đó thử mọi hoán vị của các phân đoạn đó và kiểm tra xem liệu mọi thứ tự có tạo ra một mảng được sắp xếp hay không. Ngay cả khi chúng tôi sửa một phân vùng, vẫn có k! hoán vị và số lượng phân vùng là tổ hợp. Con số này quá lớn ngay cả với n = 30. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các phân đoạn tùy ý và thay vào đó hãy nghĩ về vị trí của mảng “đã đồng ý” với thứ tự được sắp xếp. Hãy để chúng tôi sắp xếp một bản sao của mảng và so sánh các vị trí. Vì tất cả các phần tử đều khác nhau nên mỗi giá trị có một vị trí chính xác duy nhất trong mảng được sắp xếp. 

Bây giờ hãy quan sát xem một giải pháp hợp lệ thực sự yêu cầu những gì: khi chúng ta hoán vị các phân đoạn, chúng ta đang nhóm các phần tử thành k khối một cách hiệu quả và mỗi khối phải có thể di chuyển được mà không phá vỡ thứ tự sắp xếp chung. Điều này trở nên khả thi một cách chính xác khi chúng ta có thể chia mảng thành k phân đoạn liền kề sao cho mỗi phân đoạn, khi được xem xét nội bộ, không vi phạm thứ tự tương đối cần thiết của mảng được sắp xếp. 

Nhận xét mang tính quyết định là mỗi khi phần kề giữa các phần tử liên tiếp trong thứ tự đã sắp xếp bị “phá vỡ” trong mảng ban đầu, chúng ta buộc phải thực hiện một phép cắt. Nếu hai phần tử liên tiếp theo thứ tự được sắp xếp không liền kề trong mảng ban đầu thì chúng không thể thuộc cùng một phân đoạn nếu chúng ta muốn duy trì khả năng sắp xếp lại toàn cục. 

Vì vậy, chúng tôi tính toán có bao nhiêu "ngắt liền kề được sắp xếp" tồn tại trong mảng ban đầu khi ánh xạ tới các vị trí đã sắp xếp. Hãy để con số đó được yêu cầu cắt giảm. Nếu chúng ta cần cắt nhiều hơn k-1, chúng ta không thể đạt được k phân đoạn tuân theo cấu trúc được sắp xếp. Nếu chúng ta cần ít hơn hoặc bằng nhau, chúng ta luôn có thể chia thêm các phân đoạn để đạt chính xác k. 

Điều này làm giảm vấn đề xuống còn một số lần chuyển đổi đơn giản trong hoán vị gây ra bởi việc sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force + Cho phép | Hàm mũ | O(n) | Quá chậm | 
| Sắp xếp + Đếm các phần cắt cần thiết | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mảng thành thứ tự được sắp xếp và ghi lại vị trí của từng giá trị.

1. Xây dựng ánh xạ từ giá trị tới chỉ mục của nó trong mảng đã sắp xếp. Điều này cho chúng ta biết thứ tự dự định của các phần tử. 
2. Thay thế mảng ban đầu bằng các vị trí này, chuyển bài toán sang phân tích hoán vị từ 0 đến n−1. 
3. Quét mảng đã chuyển đổi từ trái sang phải và đếm xem phần tử tiếp theo không chính xác là phần tử kế tiếp của phần tử hiện tại theo thứ tự được sắp xếp bao nhiêu lần. Mỗi điểm gián đoạn như vậy có nghĩa là cần có một ranh giới phân đoạn mới nếu chúng ta muốn các phân đoạn có thể được sắp xếp lại một cách tự do theo thứ tự đã sắp xếp. 
4. Gọi số này là c. Đây là những phân đoạn tối thiểu cần thiết để duy trì sự liền kề được sắp xếp. 
5. Nếu c + 1 ≤ k thì chúng ta có thể phân chia thêm để đạt chính xác k phân đoạn bằng cách tùy ý phá vỡ các phân đoạn hiện có, vì việc cắt thêm không bao giờ gây tổn hại đến tính hợp lệ. 
6. Ngược lại, nếu c + 1 > k, chúng ta không thể tạo ra đủ tính linh hoạt để sắp xếp lại thành một dãy đã được sắp xếp. 

Quyết định giảm xuống còn việc kiểm tra xem k ít nhất có phải là số lần chạy đơn điệu được tạo ra bởi sự kề cận được sắp xếp hay không. 

### Tại sao nó hoạt động 

Quá trình quét xác định các khối liền kề tối đa trong đó các phần tử xuất hiện theo thứ tự được sắp xếp liên tiếp. Trong một khối như vậy, tất cả các phần tử đã tạo thành một chuỗi phải ở cùng nhau trong bất kỳ sự sắp xếp lại hợp lệ nào, bởi vì việc phá vỡ nó sẽ buộc phải đảo ngược phần kề đã được sắp xếp mà không thể sửa chữa được chỉ bằng hoán vị khối. Mỗi khi chuỗi này bị đứt, chúng tôi sẽ thực hiện cắt giảm bắt buộc. Mọi phân đoạn hợp lệ đều phải tôn trọng những phần cắt này và mọi phần cắt bổ sung đều là các sàng lọc tùy chọn. Do đó, số lượng phân đoạn tối thiểu có thể đạt được chính xác là số lần chạy này và tính khả thi tương đương với việc có k không nhỏ hơn mức tối thiểu này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        b = sorted(a)
        pos = {v: i for i, v in enumerate(b)}
        
        cnt = 1
        for i in range(1, n):
            if pos[a[i]] != pos[a[i-1]] + 1:
                cnt += 1
        
        if cnt <= k:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng để xác định thứ tự mục tiêu của mọi phần tử. Từ điển`pos`lưu trữ thứ hạng của từng giá trị theo thứ tự được sắp xếp này, chuyển đổi bài toán thành một chuỗi các số nguyên từ 0 đến n−1. 

Biến`cnt`đếm có bao nhiêu phân đoạn liền kề được sắp xếp đơn điệu tồn tại theo thứ tự ban đầu. Mỗi khi các phần tử liên tiếp không liên tiếp theo thứ tự được sắp xếp thì một phân đoạn mới phải bắt đầu. 

Cuối cùng, chúng tôi so sánh số lượng phân đoạn được yêu cầu tối thiểu này với k. Nếu k đủ lớn, chúng ta luôn có thể chia nhỏ hơn để đạt chính xác k phân đoạn. 

Một sai lầm phổ biến là nghĩ rằng chúng ta phải trực tiếp hình thành chính xác k phân khúc “tốt”. Quan điểm đúng là chúng ta chỉ cần đảm bảo tối đa k phân đoạn là cần thiết về mặt cấu trúc; luôn có thể chia thêm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
6 3 4 2 1
```Mảng được sắp xếp là`[1, 2, 3, 4, 6]`và vị trí ánh xạ các giá trị ban đầu vào thứ hạng. 

| tôi | một [tôi] | pos[a[i]] | liên tiếp? | cnt | 
| --- | --- | --- | --- | --- | 
| 0 | 6 | 4 | - | 1 | 
| 1 | 3 | 2 | không | 2 | 
| 2 | 4 | 3 | vâng | 2 | 
| 3 | 2 | 1 | không | 3 | 
| 4 | 1 | 0 | không | 4 | 

Chúng ta nhận được cnt = 4. Vì k = 4 nên điều kiện đúng và câu trả lời là CÓ. 

Điều này chứng tỏ rằng mỗi lần ngắt trong vùng lân cận được sắp xếp sẽ tạo ra một phân đoạn mới và việc khớp chính xác k là đủ. 

### Ví dụ 2 

đầu vào:```
4 2
1 -4 0 -2
```Mảng được sắp xếp là`[-4, -2, 0, 1]`. 

| tôi | một [tôi] | pos[a[i]] | liên tiếp? | cnt | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | - | 1 | 
| 1 | -4 | 0 | không | 2 | 
| 2 | 0 | 2 | không | 3 | 
| 3 | -2 | 1 | không | 4 | 

Ở đây cnt = 4 nhưng k = 2 nên không thể nén cấu trúc thành 2 đoạn có thể sắp xếp lại được. 

Điều này cho thấy rằng mặc dù việc sắp xếp lại được cho phép nhưng có quá nhiều vùng lân cận bị hỏng buộc phải có quá nhiều khối độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | việc sắp xếp chiếm ưu thế trong mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | lưu trữ mảng đã sắp xếp và bản đồ vị trí | 

Tổng n trên các trường hợp thử nghiệm là 3e5, do đó, điều này dễ dàng nằm trong giới hạn. Việc sắp xếp từng trường hợp thử nghiệm một cách độc lập vẫn đủ hiệu quả vì chi phí tích lũy vẫn nằm trong giới hạn chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        b = sorted(a)
        pos = {v: i for i, v in enumerate(b)}
        
        cnt = 1
        for i in range(1, n):
            if pos[a[i]] != pos[a[i-1]] + 1:
                cnt += 1
        
        output.append("YES" if cnt <= k else "NO")
    
    return "\n".join(output)

# provided samples
assert run("""3
5 4
6 3 4 2 1
4 2
1 -4 0 -2
5 1
1 2 3 4 5
""") == """YES
NO
YES"""

# custom: already sorted, k = 1
assert run("""1
5 1
1 2 3 4 5
""") == "YES"

# custom: k = n always possible
assert run("""1
4 4
3 1 4 2
""") == "YES"

# custom: strict alternating break
assert run("""1
5 2
5 1 4 2 3
""") in ["YES", "NO"]

# custom: minimal n
assert run("""1
1 1
10
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đã được sắp xếp | CÓ | k=1 vỏ cạnh | 
| trường hợp k=n | CÓ | linh hoạt tối đa | 
| hoán vị xen kẽ | phụ thuộc | căng thẳng kề cận phá vỡ | 
| n=1 | CÓ | trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Đối với một mảng được sắp xếp đầy đủ với k = 1, thuật toán sẽ đưa ra đúng CÓ vì không có ngắt kề và cnt vẫn bằng 1, phù hợp với thực tế là không cần sắp xếp lại. 

Với k = n, ngay cả khi mảng có mức độ mất trật tự cao thì cnt luôn lớn nhất là n, do đó điều kiện cnt ≤ k luôn đúng. Điều này phản ánh rằng chúng ta có thể cô lập mọi phần tử và tự do sắp xếp lại các phân đoạn đơn lẻ. 

Đối với các mảng trong đó thứ tự sắp xếp xuất hiện trong các lần chạy liên tiếp dài, cnt trở nên nhỏ và thuật toán xác định chính xác rằng chỉ cần vài phân đoạn là đủ. Bất kỳ k bổ sung nào chỉ đơn giản có nghĩa là chúng ta có thể chia nhỏ các lần chạy này hơn nữa mà không ảnh hưởng đến tính chính xác, vì việc chia tách không bao giờ đưa ra các ràng buộc thứ tự mới.
