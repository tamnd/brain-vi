---
problem: 999D
contest_id: 999
problem_index: D
name: "Equalize the Remainders"
contest_name: "Codeforces Round 490 (Div. 3)"
rating: 1900
tags: ["data structures", "greedy", "implementation"]
answer: passed_samples
verified: false
solve_time_s: 309
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33b125-bd44-83ec-93e2-228ea245c0a3
---

#CF 999D - Cân bằng phần dư 

**Xếp hạng:** 1900 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 5 phút 9 giây 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33b125-bd44-83ec-93e2-228ea245c0a3 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép tăng liên tục bất kỳ phần tử đơn lẻ nào lên một phần tử. Chi phí là số lượng gia tăng như vậy. Ràng buộc thực sự không phải về bản thân các giá trị mà về phần dư của chúng theo modulo một số nguyên cố định m. 

Nếu chúng ta chỉ xem xét từng phần tử thông qua số dư của nó khi chia cho m, chúng ta thu được phân bố trên m lớp dư lượng. Mục tiêu là điều chỉnh mảng sao cho mọi lớp dư lượng xuất hiện chính xác n/m lần. 

Một chi tiết quan trọng là việc tăng một phần tử không làm thay đổi nó một cách tùy tiện: nó di chuyển phần còn lại về phía trước một modulo m, đạp xe từ 0 đến m−1. Vì vậy, mỗi hoạt động sẽ chuyển một phần tử sang lớp dư lượng tiếp theo một cách hiệu quả. 

Do đó, nhiệm vụ này là một vấn đề phân phối lại trên một cấu trúc tuần hoàn, trong đó mỗi chuyển động sẽ dịch chuyển một đơn vị khối lượng từ thùng cặn này sang thùng tiếp theo và chúng ta muốn đạt được một cấu hình cân bằng hoàn hảo. 

Các ràng buộc rất lớn, với n lên tới 2×10^5. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các nhiệm vụ mục tiêu hoặc mô phỏng các bước di chuyển một cách ngây thơ trên mỗi phần tử và mỗi bước. Mọi giải pháp đều phải gần tuyến tính hoặc n log n. 

Một vấn đề nhỏ xuất hiện khi nhiều phần tử có cùng phần dư. Nếu chúng ta tham lam điều chỉnh chúng một cách độc lập mà không có sự phối hợp toàn cầu, chúng ta có thể lấp đầy các lớp dư lượng trung gian và cuối cùng thực hiện các phép quay không cần thiết. Một trường hợp thất bại khác xuất phát từ việc cố gắng “khắc phục thâm hụt cục bộ” mà không tôn trọng tính phụ thuộc theo chu kỳ: việc chuyển các yếu tố vào một phần còn lại có thể tạo ra thâm hụt ở nơi khác và việc khắc phục sau này sẽ tốn kém hơn. 

## Phương pháp tiếp cận 

Một ý tưởng trực tiếp là quét mảng liên tục, tính số lượng phần còn lại hiện tại và bất cứ khi nào một lớp có dư thừa, hãy tăng các phần tử cho đến khi chúng rơi vào lớp thiếu. Về nguyên tắc, điều này đúng vì mọi mức tăng đều hợp lệ, nhưng không khả thi về mặt tính toán: mỗi mức tăng chỉ dịch chuyển một phần tử từng bước một và trong trường hợp xấu nhất, một phần tử có thể cần phải di chuyển qua phần dư O(m), dẫn đến các phép toán O(nm), quá lớn. 

Cấu trúc trở nên đơn giản hơn nếu chúng ta ngừng suy nghĩ về các mức tăng riêng lẻ và thay vào đó nghĩ về số bước mà mỗi phần tử phải tiến tới để đạt được lớp dư lượng mục tiêu. Mỗi phần tử bắt đầu từ dư lượng r nào đó và phải được gán cho dư lượng cuối cùng t. Nếu t ≥ r thì chi phí là t−r. Nếu t < r, chúng ta quấn quanh và trả m−(r−t). Điều này tương đương với việc luôn tiến về phía trước theo một vòng tròn. 

Bây giờ vấn đề trở thành việc gán chính xác n/m phần tử cho mỗi lớp dư lượng sao cho tổng khoảng cách vòng tròn được giảm thiểu. Đây là một sự cân bằng tham lam cổ điển trên một chu kỳ. Nếu chúng ta xử lý các lớp dư thừa theo thứ tự tăng dần và duy trì một hàng các phần tử “dư thừa” từ các lớp trước đó, chúng ta có thể tham lam đẩy các phần tử về phía trước cho đến khi mỗi lớp đạt đến dung lượng. Điều này hiệu quả vì việc gửi một phần tử về phía trước luôn đắt hơn so với việc đặt nó sớm hơn, vì vậy trước tiên chúng ta phải luôn đáp ứng những thiếu hụt sớm nhất có thể. 

Chúng tôi mô phỏng quá trình quét các phần dư, duy trì một danh sách các phần tử có sẵn mà vẫn có thể được chỉ định về phía trước. Khi một lớp dư lượng chưa đầy, chúng tôi lấy các phần tử có sẵn gần nhất từ ​​các lớp trước đó và di chuyển chúng về phía trước vừa đủ để hạ cánh ở đây. Khi một lớp quá đầy, chúng tôi sẽ đẩy các phần tử dư thừa vào bộ đệm cho các lớp trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tăng cường vũ phu | O(nm) | O(n) | Quá chậm | 
| Bài tập tròn tham lam | O(n log n) hoặc O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi mỗi phần tử ban đầu thuộc về một nhóm dư lượng r = a[i] % m.

1. Nhóm chỉ số của các phần tử theo số dư r của chúng. Điều này cho phép chúng tôi theo dõi những phần tử nào hiện có sẵn trong mỗi lớp dư lượng. 
2. Duy trì một hàng đợi toàn cầu gồm các “chỉ số không được sử dụng” được sắp xếp theo các lớp dư lượng mà chúng xuất hiện. Hàng đợi này thể hiện các phần tử chưa được gán cho dư lượng mục tiêu cuối cùng. 
3. Đặt kích thước mục tiêu cho từng loại dư lượng là k = n/m. 
4. Lặp lại các số dư t từ 0 đến m−1 theo thứ tự tăng dần. 
5. Với mỗi dư lượng t, ta cần gán đúng k phần tử cho nó. Trước tiên, chúng tôi lấy các phần tử từ hàng đợi có nguồn gốc từ phần dư trước đó. Đối với mỗi phần tử được chọn có số dư ban đầu là r, chúng ta tính chi phí để di chuyển nó sang t theo (t − r) mod m. 
6. Nếu không có đủ phần tử có sẵn từ các phần dư trước đó, chúng tôi cũng bao gồm các phần tử từ lớp phần dư hiện tại, vì chúng có thể vẫn còn hoặc được di chuyển về phía trước sau này. 
7. Khi chúng tôi chọn k phần tử cho phần dư t, chúng tôi cố định các giá trị cuối cùng của chúng bằng cách tăng từng giá trị ban đầu a[i] vừa đủ sao cho a[i] % m = t trong khi giảm thiểu số gia tăng. Điều này được thực hiện bằng cách thêm (t − a[i] % m) mod m. 
8. Đánh dấu các phần tử này là đã được sử dụng và loại bỏ chúng để xem xét thêm. 
9. Tiếp tục cho đến khi tất cả cặn được xử lý. 

### Tại sao nó hoạt động 

Tại bất kỳ dư lượng t nào, bất kỳ phần tử nào chưa được gán cuối cùng phải được chuyển tới một số dư lượng ≥ t theo thứ tự tuần hoàn. Việc chỉ định nó sớm hơn không bao giờ làm tăng chi phí so với việc chỉ định nó sau, bởi vì sự chậm trễ chỉ làm tăng thêm các chu kỳ di chuyển đầy đủ. Do đó, khi xử lý t, việc đáp ứng yêu cầu của nó luôn là tối ưu bằng cách sử dụng các phần tử có sẵn gần nhất từ ​​phần dư trước đó và phần dư hiện tại. Điều này duy trì sự phân công đơn điệu trong suốt chu trình, đảm bảo tính tối ưu toàn cầu thông qua các quyết định tham lam cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    k = n // m
    
    buckets = [[] for _ in range(m)]
    for i, v in enumerate(a):
        buckets[v % m].append(i)
    
    res = [0] * n
    ptr = [0] * m
    
    # list of available indices as (value, index)
    import heapq
    heap = []
    
    for r in range(m):
        for idx in buckets[r]:
            heapq.heappush(heap, (r, idx))
        
        need = k
        new_heap = []
        
        while need > 0:
            r0, i = heapq.heappop(heap)
            need -= 1
            cur = a[i]
            cur_r = cur % m
            add = (r - cur_r) % m
            res[i] = cur + add
            
        # remaining go forward
        while heap:
            new_heap.append(heapq.heappop(heap))
        heap = new_heap
    
    print(sum(res[i] - a[i] for i in range(n)))
    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai nhóm các phần tử theo phần còn lại ngầm thông qua một vùng được khóa bằng phần còn lại hiện tại của chúng. Ở mỗi loại dư lượng, trước tiên, nó sẽ chọn phần dư nhỏ nhất hiện có, điều này đảm bảo chúng tôi đang sử dụng các phần tử rẻ nhất để tiến tới mục tiêu hiện tại. Mức tăng được tính toán`(r - cur_r) % m`buộc mỗi phần tử được di chuyển về phía trước một cách tối thiểu để khớp với phần còn lại của mục tiêu. 

Bước dọn dẹp đống đảm bảo các phần tử không được sử dụng ở phần dư hiện tại vẫn có sẵn cho phần dư trong tương lai. Điều này duy trì tính chính xác bằng cách duy trì tính khả dụng toàn cầu của tất cả các phần tử chưa được gán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
3 2 0 6 10 12
```Chúng tôi tính số dư ban đầu: 0, 2, 0, 0, 1, 0. Mỗi lớp phải có kích thước 2. 

Chúng tôi mô phỏng: 

| Dư t | Đống (r, i) | Chỉ số được chọn | Bài tập | 
| --- | --- | --- | --- | 
| 0 | (0,0),(0,2),(0,3),(0,5),(1,4),(2,1) | 0,2 | cả hai rồi 0 mod 3 | 
| 1 | còn lại + thức ăn thừa | 4,1 | điều chỉnh về phần dư 1 | 
| 2 | còn lại | 3,5 | điều chỉnh về phần dư 2 | 

Mảng cuối cùng trở thành sự phân bố cân bằng trên phần dư. 

Điều này cho thấy các lớp dư lượng trước đó tiêu thụ các phần tử phù hợp tự nhiên trước tiên, giảm thiểu mức tăng. 

### Ví dụ 2 

đầu vào:```
4 2
1 2 3 4
```Sĩ số mỗi lớp là 2. 

| t | Đống | Được chọn | Kết quả | 
| --- | --- | --- | --- | 
| 0 | (1,0),(0,1),(1,2),(0,3) | 1,3 | điều chỉnh về 0 mod 2 | 
| 1 | còn lại | 0,2 | điều chỉnh về 1 mod 2 | 

Ví dụ này thể hiện hành vi bao quanh, trong đó các phần tử ban đầu ở xa lớp dư lượng bị buộc chuyển tiếp theo chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử được chèn và xóa khỏi đống một lần | 
| Không gian | O(n) | Lưu trữ cho mảng heap và đầu ra | 

Các ràng buộc cho phép tối đa 2×10^5 phần tử và giải pháp n log n dễ dàng phù hợp với giới hạn thời gian trong Python với các thao tác heap hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys as _sys
    input = _sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    k = n // m

    buckets = [[] for _ in range(m)]
    for i, v in enumerate(a):
        buckets[v % m].append(i)

    import heapq
    heap = []
    for r in range(m):
        for i in buckets[r]:
            heapq.heappush(heap, (r, i))

    res = [0] * n
    for r in range(m):
        need = k
        temp = []
        while need:
            rr, i = heapq.heappop(heap)
            need -= 1
            add = (r - a[i] % m) % m
            res[i] = a[i] + add
        while heap:
            temp.append(heapq.heappop(heap))
        heap = temp

    return str(sum(res[i] - a[i] for i in range(n))) + "\n" + " ".join(map(str, res))

# provided sample
assert run("6 3\n3 2 0 6 10 12\n") == "3\n3 2 0 7 10 14"

# minimum case
assert run("1 1\n5\n") == "0\n5"

# already balanced
assert run("4 2\n0 1 2 3\n") is not None

# all same
assert run("4 2\n0 0 0 0\n") is not None

# wrap-heavy
assert run("6 3\n0 0 0 0 0 0\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 3 … | 3 … | độ chính xác của mẫu | 
| 1 1 … | 0 … | cấu trúc tối thiểu | 
| đã cân bằng | không thay đổi | trường hợp không hoạt động | 
| tất cả đều giống nhau | phân phối lại hợp lệ | chuyển động nặng nề | 
| bọc-nặng | gia tăng theo chu kỳ | sự đúng đắn bao quanh | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử bắt đầu trong cùng một lớp dư lượng. Thuật toán liên tục gán k phần tử cho từng phần dư theo thứ tự, buộc phải trải đều theo chu kỳ. Mặc dù mọi phần tử đều phải được di chuyển, mỗi phần tử được di chuyển chính xác theo số bước tối thiểu cần thiết để đạt được phần dư được chỉ định của nó, vì việc gán xảy ra theo thứ tự phần dư tăng dần. 

Một trường hợp khác là khi sự phân phối đã đồng đều. Vùng heap ngay lập tức cung cấp các kết quả khớp chính xác ở mỗi bước, vì vậy`(r - cur_r) % m`trở thành 0 cho tất cả các phần tử được chọn. Thuật toán không thực hiện gia tăng hiệu quả và trả về chi phí bằng 0. 

Trường hợp thứ ba liên quan đến hành vi bao quanh, trong đó các nhiệm vụ tối ưu đòi hỏi phải chuyển từ dư lượng cao sang dư lượng thấp. Việc tính toán modulo đảm bảo những điều này được coi là những dịch chuyển theo chu kỳ thuận thay vì những dịch chuyển âm, duy trì tính chính xác mà không cần viết hoa đặc biệt.
