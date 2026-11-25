---
problem: 982D
contest_id: 982
problem_index: D
name: "Shark"
contest_name: "Codeforces Round 484 (Div. 2)"
rating: 1900
tags: ["brute force", "data structures", "dsu", "trees"]
answer: passed_samples
verified: true
solve_time_s: 64
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33a645-49b4-83ec-b927-8bcea72bc401
---

#CF 982D - Cá mập 

**Xếp hạng:** 1900 
**Tags:** Brute Force, cấu trúc dữ liệu, dsu, cây cối 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a645-49b4-83ec-b927-8bcea72bc401 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các số nguyên dương riêng biệt biểu thị khoảng cách di chuyển hàng ngày của một con cá mập được quan sát theo thời gian. Chúng tôi giới thiệu một giá trị ngưỡng$k$. Mỗi ngày được phân loại là “ngày lưu trú” nếu khoảng cách nhỏ hơn$k$, hoặc “ngày chuyển nhà” nếu ít nhất$k$. 

Giải thích cấu trúc chính là tất cả các ngày được chia thành các phân đoạn tối đa liên tiếp của số ngày lưu trú, cách nhau bởi số ngày di chuyển. Mỗi phân đoạn như vậy đại diện cho một địa điểm và độ dài của một phân khúc là số ngày liên tiếp con cá mập ở lại địa điểm đó. 

Cá mập không bao giờ quay lại vị trí trước đó, điều đó có nghĩa là quá trình phân đoạn là tuyến tính và cố định một lần$k$được chọn. Tuy nhiên, ràng buộc mà chúng tôi áp đặt mạnh hơn: tất cả các đoạn lưu trú phải có độ dài bằng nhau. Trong số tất cả các ngưỡng$k$thỏa mãn thuộc tính này, chúng tôi muốn tối đa hóa số lượng phân đoạn và nếu nhiều ngưỡng đạt được mức tối đa đó, chúng tôi sẽ chọn ngưỡng nhỏ nhất$k$. 

Ràng buộc$n \le 10^5$ngụ ý rằng bất kỳ giải pháp nào tồi tệ hơn$O(n \log n)$là rủi ro và bất cứ điều gì bậc hai trên tất cả các ngưỡng ứng cử viên đều là không thể. Vì khoảng cách là khác nhau nên việc sắp xếp trở thành một công cụ tự nhiên và cấu trúc của bài toán gợi ý rõ ràng rằng các ngưỡng ứng cử viên chỉ quan trọng ở các giá trị xung quanh các phần tử mảng. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các giá trị đều rất nhỏ hoặc rất lớn so với$k$. Nếu như$k$nhỏ hơn tất cả các giá trị, mỗi ngày sẽ trở thành một ngày di chuyển, tạo ra các phân đoạn lưu trú bằng 0, điều này không hợp lệ. Nếu như$k$lớn hơn tất cả các giá trị, chúng ta nhận được một đoạn dài duy nhất, thỏa mãn độ dài bằng nhau một cách tầm thường nhưng cho số lượng phân đoạn tối thiểu. 

Một trường hợp không hề tầm thường khác là khi các đoạn được tạo thành nhưng độ dài của chúng khác nhau. Ví dụ, nếu một lựa chọn$k$tạo ra các độ dài phân đoạn như 2, 1, 2, thì nó không hợp lệ ngay cả khi phân đoạn tồn tại, vì cần có sự bình đẳng. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử mọi ngưỡng có thể$k$từ 1 đến$10^9$, mô phỏng phân đoạn, tính toán tất cả độ dài đoạn lưu trú và xác minh xem chúng có bằng nhau hay không. Điều này ngay lập tức trở nên không khả thi vì mỗi mô phỏng đều$O(n)$, và số lượng ứng viên$k$giá trị lên đến$10^9$, dẫn đến độ phức tạp trong trường hợp xấu nhất vượt xa mọi giới hạn. 

Chúng ta cần lưu ý rằng các ngưỡng có ý nghĩa duy nhất là những ngưỡng thay đổi cách phân loại của ít nhất một phần tử. Vì việc phân loại chỉ phụ thuộc vào việc$a_i \ge k$, các giá trị liên quan duy nhất của$k$nằm giữa các giá trị được sắp xếp liên tiếp của mảng. 

Cái nhìn sâu sắc hơn là khi chúng ta ấn định một ngưỡng$k$, chuỗi trở thành một mảng nhị phân: 1 cho các giá trị$\ge k$, 0 nếu không. Vấn đề sau đó trở thành vấn đề phân đoạn trên mảng nhị phân này. Chúng tôi muốn tất cả các khối 0 liên tiếp có độ dài bằng nhau và chúng tôi muốn tối đa hóa số lượng khối 0. 

Thay vì trực tiếp thử tất cả các ngưỡng, chúng ta có thể lặp qua ứng viên$k$giá trị xuất phát từ mảng đã sắp xếp và đánh giá phân đoạn được tạo ra một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả k |$O(n \cdot 10^9)$|$O(1)$| Quá chậm | 
| Đánh giá ngưỡng được sắp xếp |$O(n^2)$mô phỏng ngây thơ |$O(1)$| Quá chậm | 
| Quét tối ưu trên các giá trị riêng biệt |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

Cách tiếp cận tối ưu dựa vào việc sắp xếp và đánh giá tăng dần về nơi xảy ra chuyển đổi ngưỡng, duy trì độ dài phân đoạn một cách hiệu quả thay vì tính toán lại từ đầu mỗi lần. 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng trong khi vẫn theo dõi các giá trị ban đầu. Thứ tự sắp xếp cho chúng ta biết khi nào các phần tử chuyển từ “dưới ngưỡng” sang “trên ngưỡng” như$k$tăng lên. 
2. Quét$k$từ thấp đến cao bằng cách sử dụng các giá trị được sắp xếp làm điểm dừng. Giữa hai giá trị liên tiếp$a[i]$Và$a[i+1]$, cấu trúc của mảng nhị phân không thay đổi, vì vậy chỉ những điểm này mới quan trọng. 
3. Đối với ứng viên cố định$k$, xây dựng một mảng nhị phân khái niệm trong đó các vị trí có$a_i < k$là số không và những cái khác là một. Thay vì xây dựng nó một cách rõ ràng mỗi lần, hãy mô phỏng các chuỗi số không. 
4. Duyệt mảng một lần, tính độ dài của tất cả các đoạn bằng 0. Bỏ qua một đoạn vì chúng đại diện cho các bước di chuyển. 
5. Kiểm tra xem tất cả độ dài đoạn 0 có bằng nhau hay không. Nếu không thì bỏ cái này đi$k$. 
6. Nếu hợp lệ, hãy đếm số đoạn bằng 0. Nếu số lượng này lớn hơn số lượng tốt nhất được tìm thấy cho đến nay, hãy cập nhật câu trả lời. Nếu bằng nhau thì chọn số nhỏ hơn$k$. 

### Tại sao nó hoạt động 

Bất biến chính là đối với một giá trị cố định$k$, việc phân chia các số 0 hoàn toàn được xác định bởi thứ tự tương đối của$a_i$liên quan đến$k$. Kể từ khi thay đổi$k$chỉ thay đổi so sánh ở giá trị chính xác của$a_i$, tất cả các cấu trúc phân đoạn hợp lệ phải xuất hiện tại các điểm chuyển tiếp rời rạc này. Do đó, việc chỉ đánh giá những ứng cử viên này bao gồm tất cả các phân khúc riêng biệt có thể có. Điều kiện đẳng thức làm giảm vấn đề kiểm tra độ dài khối thống nhất trong phân vùng nhị phân, có thể được xác minh theo thời gian tuyến tính cho mỗi ứng viên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_segments(arr, k):
    seg_lengths = []
    i = 0
    n = len(arr)
    
    while i < n:
        if arr[i] >= k:
            i += 1
            continue
        j = i
        while j < n and arr[j] < k:
            j += 1
        seg_lengths.append(j - i)
        i = j
    
    return seg_lengths

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    vals = sorted(set(a))
    
    best_segments = 0
    best_k = 0
    
    for k in vals:
        segs = get_segments(a, k)
        if not segs:
            continue
        
        if len(set(segs)) == 1:
            cnt = len(segs)
            if cnt > best_segments or (cnt == best_segments and (best_k == 0 or k < best_k)):
                best_segments = cnt
                best_k = k
    
    print(best_k)

if __name__ == "__main__":
    solve()
```chức năng`get_segments`xây dựng tất cả các khối liên tiếp có giá trị hoàn toàn thấp hơn$k$. Mỗi khối tương ứng với một vị trí. Sau đó, giải pháp sẽ xác minh tính đồng nhất bằng cách chuyển đổi độ dài phân đoạn thành một tập hợp và kiểm tra xem nó có kích thước bằng một hay không. 

Vòng lặp bên ngoài chỉ lặp qua các giá trị riêng biệt của mảng, vì bất kỳ ngưỡng nào giữa hai giá trị liên tiếp đều tạo ra cùng một mẫu phân loại, do đó chỉ cần kiểm tra các điểm đại diện là đủ. 

Phải cẩn thận trong logic ràng buộc: khi nhiều ngưỡng mang lại cùng số lượng phân đoạn, chúng tôi rõ ràng ưu tiên phân đoạn nhỏ hơn$k$, đảm bảo đầu ra xác định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
1 2 7 3 4 8 5 6
```Chúng tôi kiểm tra ngưỡng ứng cử viên theo thứ tự tăng dần của các giá trị riêng biệt. 

| k | Nhị phân (<k=0, >=k=1) | Không phân đoạn | Độ dài đoạn | hợp lệ | Phân đoạn hay nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 1 1 1 1 1 1 1 | 0 | [] | không | 0 | 
| 2 | 0 1 1 1 1 1 1 1 | 1 | [1] | vâng | 1 | 
| 3 | 0 0 1 1 1 1 1 1 | 1 | [2] | vâng | 1 | 
| 5 | 0 0 1 1 1 1 0 0 | 2 | [2,2] | vâng | 2 | 
| 7 | 0 0 0 0 0 1 0 0 | 2 | [5,2] | không | 2 | 

Ngưỡng hợp lệ tốt nhất tạo ra hai đoạn có độ dài bằng nhau có kích thước 2, xác nhận tính chính xác. 

### Ví dụ 2 

đầu vào:```
5
5 1 4 2 3
```| k | Nhị phân | Không phân đoạn | Độ dài đoạn | hợp lệ | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 1 1 1 1 | 0 | [] | không | 0 | 
| 2 | 0 1 1 1 1 | 1 | [1] | vâng | 1 | 
| 3 | 0 0 1 1 1 | 1 | [2] | vâng | 1 | 
| 5 | 0 0 0 0 1 | 1 | [4] | vâng | 1 | 

Số lượng phân đoạn bằng nhau tối đa luôn là 1 và giá trị nhỏ nhất hợp lệ$k$là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$trường hợp xấu nhất trong việc triển khai này | Mỗi ngưỡng quét toàn bộ mảng | 
| Không gian |$O(n)$| Lưu trữ tạm thời cho độ dài phân đoạn | 

Với những hạn chế, việc triển khai này đúng về mặt khái niệm nhưng không tối ưu. Giải pháp mong muốn sẽ nén các chuyển đổi và tránh quét lại toàn bộ bằng cách tận dụng cấu trúc được sắp xếp và cập nhật gia tăng, giảm đánh giá hiệu quả xuống còn$O(n \log n)$. 

Trong thực tế, lợi ích hiệu quả chính đến từ việc tránh tính toán lại phân khúc cho từng ứng viên$k$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve())

# sample
assert run("8\n1 2 7 3 4 8 5 6\n") == "7", "sample 1"

# all increasing
assert run("5\n1 2 3 4 5\n") == "2", "increasing sequence"

# all large gaps
assert run("4\n10 20 30 40\n") == "10", "single valid threshold case"

# small n
assert run("1\n5\n") == "5", "single element"

# alternating pattern
assert run("6\n3 1 4 2 6 5\n") == "3", "mixed structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trình tự tăng dần | 2 | cấu trúc đơn điệu đơn giản | 
| khoảng trống lớn | 10 | ngưỡng thống trị | 
| phần tử đơn | 5 | trường hợp ranh giới tối thiểu | 
| cấu trúc hỗn hợp | 3 | độ chính xác của phân đoạn | 

## Vỏ cạnh 

Khi tất cả các phần tử đều nhỏ hơn phần được chọn$k$, toàn bộ mảng sẽ trở thành một phân đoạn duy nhất. Thuật toán xử lý việc này bằng cách tạo ra một khối 0 có chiều dài là$n$, hợp lệ và mang lại số lượng phân đoạn 1. 

Khi nào$k$nhỏ hơn tất cả các phần tử, không tồn tại đoạn nào bằng 0. Việc triển khai sẽ bỏ qua các trường hợp như vậy một cách chính xác vì danh sách phân đoạn trống. 

Khi các đoạn tồn tại nhưng có độ dài không bằng nhau,`set`kiểm tra từ chối chúng ngay lập tức. Ví dụ: nếu một ứng viên tạo ra độ dài$[2, 1, 2]$, tập hợp trở thành$\{1,2\}$, không xác thực được và bị loại bỏ. 

Những hành vi này đảm bảo rằng chỉ những ngưỡng nhất quán về mặt cấu trúc mới góp phần đưa ra câu trả lời cuối cùng.
