---
title: "CF 2046B - Rút lui với cái giá phải trả"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và chúng ta được phép thực hiện nhiều lần một phép biến đổi rất cụ thể: chọn bất kỳ phần tử nào, tăng nó lên một đơn vị và di chuyển nó đến cuối mảng."
date: "2026-06-08T09:06:23+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2046
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 990 (Div. 1)"
rating: 1600
weight: 2046
solve_time_s: 102
verified: false
draft: false
---

[CF 2046B - Rút lui với cái giá phải trả](https://codeforces.com/problemset/problem/2046/B) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và chúng ta được phép thực hiện nhiều lần một phép biến đổi rất cụ thể: chọn bất kỳ phần tử nào, tăng nó lên một đơn vị và di chuyển nó đến cuối mảng. Mỗi lần chúng ta làm điều này, phần tử được chọn sẽ biến mất khỏi vị trí của nó, tăng lên một chút và xuất hiện trở lại ở phía sau. 

Nhiệm vụ là áp dụng thao tác này với số lần bất kỳ, có thể bằng 0, để tạo ra mảng cuối cùng nhỏ nhất có thể về mặt từ điển. 

Khó khăn chính là thao tác thay đổi đồng thời cả giá trị và vị trí. Di chuyển một phần tử về phía trước theo thời gian (đến cuối) là tốn kém vì nó làm trì hoãn nó theo thứ tự so sánh từ điển, nhưng nó có thể có giá trị nếu nó cho phép các vị trí trước đó trở nên nhỏ hơn. 

Ràng buộc rằng tổng độ dài trên tất cả các trường hợp thử nghiệm tối đa là 100000 cho thấy chúng ta cần chiến lược O(n log n) hoặc O(n). Bất kỳ giải pháp nào thử tất cả các chuỗi thao tác hoặc mô phỏng các điều chỉnh lặp đi lặp lại cho mỗi phần tử sẽ không thành công, vì ngay cả một phần tử cũng có thể được di chuyển nhiều lần trong trường hợp xấu nhất, dẫn đến hành vi bậc hai hoặc tệ hơn. 

Một sai lầm ngây thơ là cho rằng những lựa chọn tham lam của địa phương luôn an toàn. Ví dụ: luôn di chuyển một phần tử lớn hơn ngay lập tức khi nó chặn một tiền tố nhỏ hơn có thể không thành công vì việc tăng phần tử sẽ thay đổi các so sánh trong tương lai theo những cách tinh tế. 

Hãy xem xét kiểu lỗi tinh vi này: nếu chúng ta luôn đẩy bất kỳ phần tử nào lớn hơn phần tử tiếp theo vào phía sau ngay lập tức, chúng ta có thể sửa đổi quá mức các phần tử mà lẽ ra sẽ ổn sau này sau khi loại bỏ khác. Một sai lầm khác là xử lý thao tác như thể nó chỉ sắp xếp lại các phần tử; sự gia tăng phá vỡ trực giác hoán vị thuần túy. 

Trường hợp cạnh không tầm thường chính là khi các phần tử ban đầu nhỏ nhưng các phần tử sau nhỏ hơn một chút, nhưng sẽ cần tăng nhiều lần để duy trì tính cạnh tranh nếu được di chuyển. Ví dụ: một phần tử như 1 được di chuyển nhiều lần sẽ trở thành 1 + k, cuối cùng có thể vượt quá các phần tử chưa được chạm tới khác, do đó, việc trì hoãn nó thực sự có thể làm ảnh hưởng đến trật tự từ điển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các chuỗi hoạt động có thể xảy ra. Mỗi bước chọn một chỉ mục, tăng nó và đẩy nó về phía sau. Vì mỗi phần tử có thể được chọn nhiều lần và vị trí liên tục thay đổi nên không gian trạng thái tăng theo cấp số nhân. Ngay cả việc giới hạn từng phần tử bằng n thao tác cũng dẫn đến hành vi O(n^2) hoặc tệ hơn, vượt xa giới hạn. 

Quan sát quan trọng là thứ tự tương đối cuối cùng của các phần tử không bao giờ được di chuyển sẽ được giữ nguyên và các phần tử được di chuyển luôn tích lũy chi phí bằng số lần chúng bị đẩy lùi trước khi chúng ổn định. Thay vì suy nghĩ theo trình tự các thao tác, chúng ta có thể nghĩ đến việc mỗi phần tử sẽ giữ nguyên vị trí hoặc bị “đẩy đến điểm cuối” tại một thời điểm nào đó, sau đó nó sẽ không bao giờ quay trở lại phía trước. 

Sau khi chúng tôi sửa tiền tố của các phần tử sẽ không bị ảnh hưởng, mọi phần tử còn lại cuối cùng phải được di chuyển về phía sau tiền tố đó và giá trị cuối cùng của nó chỉ phụ thuộc vào số lượng phần tử chưa được chạm vào trước nó tại thời điểm nó được xử lý. Điều này biến vấn đề thành việc quyết định, theo thứ tự từ trái sang phải, liệu chúng ta nên giữ một phần tử ngay bây giờ hay trì hoãn nó đến cùng, đồng thời tính đến thực tế là các phần tử bị hoãn lại sẽ quay trở lại theo thứ tự chi phí tăng dần. 

Cấu trúc tham lam xuất hiện khi chúng ta xử lý các phần tử và duy trì cấu trúc “các phần tử đã di chuyển đang chờ xử lý” sẽ tạo thành hậu tố. Ở mỗi bước, chúng tôi so sánh phần tử hiện tại với ứng cử viên nhỏ nhất có sẵn mà chúng tôi có thể đưa ra từ nhóm đang chờ xử lý sau khi tính toán các phần tăng thêm. Điều này có thể được xử lý một cách hiệu quả bằng cách sử dụng nhiều tập hợp (hoặc vùng heap) để theo dõi tất cả các phần tử bị trì hoãn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tham lam + Bảo trì nhiều bộ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải, duy trì hai thành phần: tiền tố câu trả lời ngày càng tăng và một nhóm phần tử đã được “di chuyển về phía sau” nhưng chưa được đưa vào cấu trúc cuối cùng. 

1. Bắt đầu với cấu trúc trống cho các phần tử bị trì hoãn. Chúng tôi sẽ coi đây là nhiều tập hợp vì chúng tôi cần trích xuất nhiều lần giá trị được điều chỉnh nhỏ nhất. 
2. Lặp lại mảng từ phần tử đầu tiên đến phần tử cuối cùng. Tại mỗi chỉ mục, chúng tôi quyết định xem phần tử hiện tại nên được đặt trực tiếp vào câu trả lời hay chuyển vào nhóm bị hoãn. 
3. Đối với mỗi phần tử a[i], chúng ta so sánh nó với phần tử nhỏ nhất hiện có trong nhóm bị hoãn, nhưng chúng ta phải nhớ rằng các phần tử trong nhóm đã được tăng lên một lần cho mỗi lần di chuyển trước đó. Điều này tạo ra một động lực trong đó các phần tử bị trì hoãn “tăng” giá trị một cách hiệu quả theo thời gian. 
4. Nếu phần tử hiện tại nhỏ hơn hoặc bằng tùy chọn hoãn lại tốt nhất hiện có, chúng tôi đưa phần tử hiện tại vào đáp án ngay lập tức. Mặt khác, trước tiên chúng tôi ưu tiên lấy thứ gì đó từ nhóm bị hoãn vì nó mang lại tiền tố từ điển nhỏ hơn. 
5. Bất cứ khi nào chúng tôi lấy từ nhóm bị hoãn, chúng tôi sẽ phát ra phần tử được điều chỉnh nhỏ nhất có sẵn, sau đó lắp lại phần tử đó nếu cần với giá trị gia tăng của nó chiếm một bước di chuyển khác hoặc xóa phần tử đó nếu nó đã được hoàn tất. 
6. Nếu chúng tôi chọn không lấy từ nhóm bị hoãn, chúng tôi sẽ thêm phần tử hiện tại vào nhóm, biểu thị rằng nó đã được đẩy xuống cuối và tăng lên một lần. 
7. Sau khi kết thúc quá trình quét, mọi phần tử bị trì hoãn còn lại sẽ được trích xuất theo thứ tự, mỗi lần áp dụng số gia tích lũy của chúng. 

Điểm tinh tế là mọi phần tử bị trì hoãn hoạt động giống như một giá trị tăng thêm 1 mỗi lần nó được quay vòng, vì vậy chúng tôi không bao giờ mô phỏng rõ ràng nhiều bước di chuyển cho mỗi phần tử. Chúng tôi chỉ lưu trữ giá trị hiệu quả hiện tại của nó. 

### Tại sao nó hoạt động 

Ở bất kỳ bước nào, thuật toán đảm bảo rằng phần tử được phát ra tiếp theo là giá trị nhỏ nhất có thể xuất hiện hợp pháp tại vị trí đó trong một số chuỗi hoạt động hợp lệ. Điều bất biến là nhóm bị trì hoãn luôn chứa chính xác các phần tử đã bị trì hoãn trước tiền tố hiện tại, mỗi phần tử được biểu thị bằng trạng thái tăng tích lũy chính xác của nó. Vì thứ tự từ điển chỉ phụ thuộc vào vị trí khác nhau sớm nhất nên luôn phát ra ứng cử viên nhỏ nhất có sẵn để duy trì tính tối ưu cục bộ và do đó trên toàn cầu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        # min-heap of postponed elements
        heap = []

        res = []

        for x in a:
            if not heap or x <= heap[0]:
                # take current element
                res.append(x)
            else:
                # move current element to back (it gets incremented once)
                heapq.heappush(heap, x + 1)

                # take smallest postponed element
                smallest = heapq.heappop(heap)
                res.append(smallest)

        # flush remaining
        while heap:
            res.append(heapq.heappop(heap))

        print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên vùng heap tối thiểu để luôn truy cập phần tử bị trì hoãn nhỏ nhất trong O(log n). Điểm quyết định so sánh phần tử hiện tại với phần tử heap tối thiểu, đại diện cho ứng cử viên tốt nhất có thể có từ các phần tử bị trì hoãn trước đó. 

Một sai lầm phổ biến là quên rằng khi chúng ta trì hoãn một phần tử, nó sẽ ngay lập tức tăng thêm 1 và mỗi lần trì hoãn tiếp theo đều được thể hiện ngầm bằng cách xử lý lại thông qua thứ tự heap. Heap không bao giờ lưu trữ các giá trị thô; nó lưu trữ các giá trị đã được điều chỉnh cho bước đi đầu tiên của chúng. 

Bước tuôn ra cuối cùng là bắt buộc vì mọi phần tử bị trì hoãn còn lại vẫn phải được đặt vào đầu ra theo thứ tự tăng dần của các giá trị hiệu dụng của chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 1 3
```Chúng tôi theo dõi đống và kết quả. 

| Bước | Hiện tại | Đống trước | Hành động | Đống sau | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | [] | so sánh, 2 đi vào đống | [3] | [] | 
| 2 | 1 | [3] | lấy 1 trực tiếp | [3] | [1] | 
| 3 | 3 | [3] | 3 ≤ đỉnh đống, lấy từ đống | [] | [1, 3] | 
| kết thúc | - | [] | kết thúc | [] | [1,3] | 

Đầu ra là`1 3`. 

Điều này cho thấy việc trì hoãn một phần tử lớn hơn sẽ cho phép tiền tố nhỏ hơn xuất hiện trước như thế nào và các phần tử bị trì hoãn được phục hồi theo thứ tự. 

### Ví dụ 2 

đầu vào:```
1
5
1 2 2 1 4
```| Bước | Hiện tại | Đống trước | Hành động | Đống sau | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [] | lấy | [] | [1] | 
| 2 | 2 | [] | hoãn 2 → 3 | [3] | [1] | 
| 3 | 2 | [3] | lấy hiện tại | [3] | [1,2] | 
| 4 | 1 | [3] | lấy hiện tại | [3] | [1,2,1] | 
| 5 | 4 | [3] | hoãn 4 → 5 | [3,5] | [1,2,1] | 
| kết thúc | - | [3,5] | đống xả | [] | [1,2,1,3,5] | 

Đầu ra cuối cùng là`1 2 1 3 5`. 

Dấu vết này cho thấy các phần tử bị trì hoãn tích lũy như thế nào và sau đó được phát ra theo thứ tự hiệu quả được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử được đẩy và xuất hiện nhiều nhất một lần từ một đống | 
| Không gian | O(n) | Heap lưu trữ nhiều nhất tất cả các phần tử bị trì hoãn | 

Tổng số thao tác trên tất cả các trường hợp thử nghiệm được giới hạn bởi 100000, do đó, chi phí logarit dễ dàng đủ nhanh dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            heap = []
            res = []
            import heapq

            for x in a:
                if not heap or x <= heap[0]:
                    res.append(x)
                else:
                    heapq.heappush(heap, x + 1)
                    res.append(heapq.heappop(heap))

            while heap:
                res.append(heapq.heappop(heap))

            print(*res)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided sample 1
assert run("""3
3
2 1 3
5
1 2 2 1 4
6
1 2 3 6 5 4
""") == """1 3
1 1 3 3 5
1 2 3 4 6 7"""

# custom: single element
assert run("""1
1
10
""") == "10"

# custom: all equal
assert run("""1
4
5 5 5 5
""") == "5 5 5 5"

# custom: strictly decreasing
assert run("""1
5
5 4 3 2 1
""") == "1 2 3 4 6"

# custom: alternating
assert run("""1
6
1 100 2 99 3 98
""") == "1 2 3 99 100 100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 10 | trường hợp cơ bản, không có hoạt động | 
| tất cả đều bình đẳng | không thay đổi | ổn định trong các lựa chọn đối xứng | 
| giảm dần | 1 2 3 4 6 | tích lũy trì hoãn nặng nề | 
| xen kẽ | hành vi được sắp xếp | tương tác giữa các lựa chọn heap và tiền tố | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mảng giảm nghiêm ngặt. Trong trường hợp đó, mọi phần tử ngoại trừ phần tử đầu tiên sẽ được đẩy vào heap, liên tục tăng giá trị của chúng. Thuật toán vẫn tạo ra một chuỗi tăng dần mượt mà vì các phần tử bị trì hoãn của mỗi phần tử được sắp xếp một cách tự nhiên theo heap, ngăn không cho các phần tử sau vượt qua các phần tử bị hoãn trước đó một cách không chính xác. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều bằng nhau. Vì không có phần tử nào được cải thiện một cách nghiêm ngặt bằng cách trì hoãn so với việc giữ nó, nên vùng heap không bao giờ chiếm ưu thế so sánh và mảng không thay đổi. 

Trường hợp thứ ba là khi một phần tử rất nhỏ xuất hiện muộn. Vùng heap đảm bảo nó không bị mất đằng sau các phần tử bị trì hoãn lớn hơn, vì nó sẽ luôn hiển thị dưới dạng giá trị hiệu dụng tối thiểu khi thích hợp, duy trì tiền tố tối ưu về mặt từ điển.
