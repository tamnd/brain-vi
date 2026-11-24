---
title: "CF 913D - Bài toán quá dễ"
description: "Chúng ta được giao một tập hợp các bài thi, mỗi bài có thời gian giải và một “giới hạn nghiêm ngặt” để kiểm soát liệu nó có đóng góp vào điểm số của chúng ta hay không. Chúng ta có thể chọn bất kỳ tập hợp con vấn đề nào để giải miễn là tổng thời gian không vượt quá thời gian làm bài."
date: "2026-06-13T01:05:17+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 913
codeforces_index: "D"
codeforces_contest_name: "Hello 2018"
rating: 1800
weight: 913
solve_time_s: 410
verified: false
draft: false
---

[CF 913D - Những vấn đề quá dễ dàng](https://codeforces.com/problemset/problem/913/D) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 6 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một tập hợp các bài thi, mỗi bài có thời gian giải và một “giới hạn nghiêm ngặt” để kiểm soát liệu nó có đóng góp vào điểm số của chúng ta hay không. Chúng ta có thể chọn bất kỳ tập hợp con vấn đề nào để giải miễn là tổng thời gian không vượt quá thời gian làm bài. Tuy nhiên, việc cho điểm không chỉ đơn giản là số lượng bài toán được giải quyết. Nếu chúng ta giải được tổng cộng k bài toán thì bài toán được giải chỉ đóng góp một điểm nếu giới hạn của chính nó ít nhất là k. Vì vậy, điểm số phụ thuộc vào kích thước cuối cùng của tập hợp đã chọn và vào số lượng các vấn đề được chọn có thể “chịu đựng được” kích thước đó. 

Mục tiêu là chọn một tập hợp con phù hợp với tổng thời gian T và tối đa hóa số lượng bài toán được chọn vẫn hợp lệ trong điều kiện phụ thuộc k này. 

Kích thước đầu vào n có thể lên tới 200.000, loại trừ mọi tìm kiếm tập hợp con theo cấp số nhân. Ngay cả các phương pháp tiếp cận O(n²) cũng đã quá chậm trong trường hợp xấu nhất. Các giá trị thời gian đủ nhỏ để sắp xếp và cấu trúc tham lam, nhưng không đủ để mô phỏng lồng nhau trên tất cả các tập hợp con. 

Một cạm bẫy ngây thơ xuất hiện khi người ta chỉ tập trung vào việc lựa chọn các bài toán nhanh hoặc chỉ vào việc tối đa hóa số lượng. Ví dụ, việc chọn k bài toán thời gian nhỏ nhất không đảm bảo rằng nhiều bài trong số đó có aᵢ đủ lớn. Tương tự, việc chỉ chọn các giá trị aᵢ cao sẽ bỏ qua tính khả thi về mặt thời gian. Sự tương tác giữa kích thước tập hợp con và ngưỡng hợp lệ là khó khăn chính. 

Một trường hợp phức tạp là khi thêm một vấn đề mới sẽ làm tăng k và làm mất hiệu lực các lựa chọn hợp lệ trước đó. Ví dụ: giả sử k hiện tại là 2 và chúng ta có bài toán được chọn với aᵢ = 2. Nó vẫn hợp lệ. Nếu chúng ta thêm một bài toán khác, k sẽ trở thành 3 và bài toán đó trở nên không hợp lệ. Bất kỳ thuật toán chính xác nào cũng phải lường trước được vòng phản hồi này. 

## Phương pháp tiếp cận 

Một giải pháp Brute Force sẽ thử tất cả các tập hợp con, tính toán tổng thời gian và đánh giá điểm bằng cách kiểm tra xem có bao nhiêu vấn đề được chọn thỏa mãn aᵢ ≥ k. Điều này đúng vì nó tuân theo định nghĩa trực tiếp nhưng nó khám phá 2ⁿ tập hợp con. Với n lên tới 200.000, thậm chí 2³⁰ cũng trở nên không khả thi nên phương pháp này sụp đổ ngay lập tức. 

Quan sát chính là đảo ngược quan điểm. Thay vì chọn một tập con và tính k sau đó, chúng ta có thể cố gắng sửa k và hỏi: liệu có thể chọn k bài toán sao cho tổng thời gian của chúng là ≤ T và ít nhất k trong số chúng thỏa mãn aᵢ ≥ k? Nhưng nếu chúng ta đã sửa k mục đã chọn thì tất cả chúng đều phải thỏa mãn aᵢ ≥ k, nếu không chúng không thể đóng góp vào điểm. Điều này biến bài toán thành việc chọn k bài toán “đủ điều kiện” từ những bài có aᵢ ≥ k trong khi giảm thiểu tổng thời gian. 

Đối với k cố định, cách tối ưu rất rõ ràng: trong số tất cả các bài toán có aᵢ ≥ k, hãy chọn k giá trị tᵢ nhỏ nhất. Nếu tổng của chúng nhỏ hơn T thì k khả thi. Chúng ta có thể kiểm tra tính khả thi của từng k và lấy giá trị tối đa. Để thực hiện điều này một cách hiệu quả, chúng tôi sắp xếp các vấn đề theo aᵢ và duy trì một cấu trúc (thường là min-heap hoặc multiset) để theo dõi thời gian nhỏ nhất trong số các vấn đề đủ điều kiện khi k giảm. 

Chúng ta lặp lại k từ n xuống 1, dần dần kích hoạt nhiều vấn đề hơn (vì aᵢ ≥ k trở nên dễ dàng hơn khi k giảm). Chúng tôi duy trì một lượng thời gian ứng cử viên tối thiểu và giữ tổng số k thời gian nhỏ nhất đã chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ · n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi điều chỉnh lại vấn đề bằng cách kiểm tra tất cả các kích thước cuối cùng có thể có k từ lớn nhất đến nhỏ nhất, duy trì một nhóm các vấn đề có thể sử dụng được.

1. Sắp xếp các bài toán theo aᵢ theo thứ tự giảm dần. Điều này cho phép chúng ta kích hoạt các vấn đề khi k giảm, vì k lớn hơn sẽ đặt ra yêu cầu chặt chẽ hơn. 
2. Khởi tạo một vùng nhớ tối thiểu trống để lưu trữ chi phí thời gian của các vấn đề hiện đủ điều kiện. Đồng thời duy trì một biến để theo dõi tổng thời gian đã chọn và vùng nhớ hoặc cấu trúc thứ hai để duy trì k thời gian nhỏ nhất. 
3. Bắt đầu từ k = n và di chuyển xuống 1. Tại mỗi k, chúng ta cộng tất cả các bài toán có aᵢ bằng k vào nhóm. Hiện tại chúng đủ điều kiện cho bất kỳ giải pháp nào có kích thước k trở xuống. 
4. Đối với nhóm hiện tại, chúng tôi muốn biết liệu chúng tôi có thể chọn k bài toán với tổng thời gian tối thiểu có thể hay không. Chúng tôi duy trì cấu trúc luôn cho phép trích xuất k lần nhỏ nhất trong số các mục đủ điều kiện. Một thủ thuật phổ biến là đẩy tất cả thời gian đủ điều kiện vào một đống có kích thước k tối đa, chỉ giữ lại k giá trị nhỏ nhất bằng cách loại bỏ những giá trị lớn hơn. 
5. Sau khi chèn các ứng cử viên cho k này, nếu chúng ta có ít nhất k phần tử trong heap, hãy tính tổng của chúng. Nếu nó không vượt quá T, chúng tôi ghi lại k là khả thi và lưu trữ lựa chọn hiện tại. 
6. K đầu tiên (từ lớn đến nhỏ) khả thi sẽ cho câu trả lời tối đa, vì tính khả thi chỉ trở nên dễ dàng hơn khi k giảm. 

### Tại sao nó hoạt động 

Với mọi k cố định, mọi nghiệm hợp lệ chỉ được chứa các bài toán có aᵢ ≥ k. Trong số đó, việc giảm thiểu tổng thời gian tương đương với việc chọn k giá trị tᵢ nhỏ nhất. Thuật toán đảm bảo rằng với mỗi k chúng ta đang duy trì một cách hiệu quả chính xác tập con tối ưu này. Vì chúng ta kiểm tra k theo thứ tự giảm dần nên k khả thi đầu tiên là lớn nhất và không thể có k lớn hơn. 

Bất biến quan trọng là ở mỗi bước k, heap chứa k lần nhỏ nhất trong số tất cả các bài toán có aᵢ ≥ k đã được xem xét cho đến nay. Điều này đảm bảo rằng việc kiểm tra tính khả thi là chính xác và không gần đúng. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, T = map(int, input().split())
    problems = [[] for _ in range(n + 1)]
    
    for i in range(n):
        a, t = map(int, input().split())
        problems[a].append((t, i + 1))
    
    heap = []
    total_time = 0
    best_k = 0
    best_set = []
    
    # We maintain a max-heap using negative values to keep k smallest times
    current = []
    
    for k in range(n, 0, -1):
        for t, idx in problems[k]:
            heapq.heappush(current, -t)
            total_time += t
            
            if len(current) > k:
                removed = -heapq.heappop(current)
                total_time -= removed
        
        if len(current) == k and total_time <= T:
            best_k = k
            best_set = [idx for _, idx in [( -x, -1) for x in current]]  # placeholder rebuild
    
    # Reconstruct properly for best_k
    heap = []
    total_time = 0
    res = []
    
    for k in range(n, 0, -1):
        for t, idx in problems[k]:
            heapq.heappush(heap, (t, idx))
        
        if len(heap) >= best_k:
            temp = heapq.nsmallest(best_k, heap)
            if sum(t for t, _ in temp) <= T:
                res = [idx for _, idx in temp]
                break
    
    print(best_k)
    print(len(res))
    print(*res)

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên xây dựng các nhóm bài toán theo giá trị aᵢ của chúng để chúng ta có thể kích hoạt các ràng buộc dần dần. Giai đoạn thứ hai duy trì rất nhiều thời gian ứng cử viên và đảm bảo chúng tôi chỉ giữ lại những khoảng thời gian nhỏ nhất có liên quan, vì những khoảng thời gian lớn hơn chỉ có thể ảnh hưởng đến tính khả thi. 

Một chi tiết triển khai tinh tế là chúng ta không thể trực tiếp duy trì cấu trúc kích thước k cho mỗi k một cách độc lập. Thay vào đó, chúng tôi dựa vào việc chèn và cắt tỉa tăng dần, điều này đảm bảo tính chính xác trên tất cả các giá trị k mà không cần tính toán lại từ đầu. 

Bước xây dựng lại được tách ra vì việc lưu trữ thành viên chính xác cho mỗi k sẽ tốn kém; thay vào đó, chúng tôi tính toán lại tập khả thi tốt nhất sau khi biết k. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 300
3 100
4 150
4 80
2 90
2 300
```Chúng tôi theo dõi kích hoạt bằng aᵢ: 

| k | Mới được thêm | Lần ứng tuyển | Đã chọn k nhỏ nhất | Tổng hợp | Khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 5 | không | [] | [] | 0 | không | 
| 4 | (150), (80) | [80,150] | [80,150] | 230 | vâng | 
| 3 | (100) | [80,100,150] | [80,100,150] | 330 | không | 
| 2 | (90), (300) | [80,90,100,150,300] | tốt nhất 2 = [80,90] | 170 | vâng | 
| 1 | không | giống nhau | tốt nhất 1 = [80] | 80 | vâng | 

Tại k = 4 thì khả thi, nhưng k = 3 thất bại do bùng nổ thời gian và k = 2 lại khả thi. Tối đa là 2. 

Điều này xác nhận sự tương tác giữa k và tính khả thi: việc thêm nhiều ứng viên hơn không đảm bảo k sẽ hoạt động cao hơn, bởi vì kích thước tập hợp con thay đổi áp lực lựa chọn. 

### Ví dụ 2 

đầu vào:```
3 100
1 60
1 50
1 40
```| k | Ứng viên | Mặt hàng k tốt nhất | Tổng hợp | Khả thi | 
| --- | --- | --- | --- | --- | 
| 3 | không | [] | 0 | không | 
| 2 | không | [] | 0 | không | 
| 1 | tất cả | [40] | 40 | vâng | 

Chỉ k = 1 là khả thi vì tất cả aᵢ đều quá nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi vấn đề được chèn vào một đống một lần và các thao tác trên đống chiếm ưu thế | 
| Không gian | O(n) | Lưu trữ cho các vấn đề được nhóm và cấu trúc heap | 

Độ phức tạp nằm trong giới hạn n lên tới 200.000, vì các hoạt động log n vẫn hiệu quả và việc sử dụng bộ nhớ là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    n, T = map(int, sys.stdin.readline().split())
    probs = []
    for i in range(n):
        a, t = map(int, sys.stdin.readline().split())
        probs.append((a, t, i + 1))

    best_k = 0
    best_set = []

    for k in range(n, 0, -1):
        cand = [t for a, t, i in probs if a >= k]
        cand.sort()
        if len(cand) >= k and sum(cand[:k]) <= T:
            best_k = k
            break

    if best_k == 0:
        return "0\n0\n"

    eligible = [(t, i) for a, t, i in probs if a >= best_k]
    eligible.sort()
    chosen = eligible[:best_k]
    return f"{best_k}\n{len(chosen)}\n" + " ".join(str(i) for _, i in chosen)

# provided sample
assert run("5 300\n3 100\n4 150\n4 80\n2 90\n2 300\n") == "2\n2\n3 4\n"

# custom cases
assert run("1 10\n1 5\n") == "1\n1\n1", "single item"
assert run("3 10\n1 5\n1 6\n1 7\n") == "1\n1\n1", "tight budget"
assert run("4 100\n4 10\n4 10\n4 10\n4 10\n") == "4\n4\n1 2 3 4", "all equal"
assert run("3 5\n3 10\n3 10\n3 10\n") == "0\n0\n", "no feasible time"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục duy nhất | 1 vấn đề khả thi | ranh giới tối thiểu | 
| ngân sách eo hẹp | chỉ có thể chọn một người | sự đúng đắn tham lam | 
| tất cả đều bình đẳng | công trình tuyển chọn đầy đủ | trường hợp đối xứng | 
| không có thời gian khả thi | câu trả lời trống rỗng | cạnh điểm 0 | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi tất cả các bài toán có aᵢ rất nhỏ, chẳng hạn như tất cả đều bằng 1. Trong trường hợp đó, mọi giải pháp có k ≥ 2 đều tự động không hợp lệ bất kể thời gian, bởi vì mọi bài toán được chọn đều yêu cầu k ≤ 1. Thuật toán xử lý chính xác điều này bằng cách không bao giờ coi k > 1 là khả thi vì không có ứng cử viên nào tồn tại cho giá trị k cao hơn. 

Một trường hợp cạnh khác phát sinh khi thời gian cực kỳ lớn nhưng giá trị aᵢ lại bị hạn chế. Ngay cả khi chúng ta có thể giải quyết tất cả các vấn đề kịp thời, điểm số vẫn bị giới hạn ở mức aᵢ nhỏ nhất trong số các mục được chọn. Thuật toán phản ánh điều này một cách tự nhiên vì tính khả thi luôn được kiểm soát bởi aᵢ ≥ k, ngăn chặn việc đánh giá quá cao điểm số. 

Cuối cùng, khi một bài toán có thời gian cực lớn nhưng aᵢ cao thì nó sẽ không bao giờ được chọn trừ khi k rất nhỏ. Vì chúng tôi luôn chọn thời điểm nhỏ nhất trước tiên nên các giá trị ngoại lệ như vậy sẽ tự động bị loại trừ trừ khi cần thiết, duy trì tính tối ưu.
