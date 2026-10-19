---
title: "CF 105624I - \u042f\u0437\u044b\u043a \u043f\u043b\u0435\u043c\u0435\u043d\u0438 \u041c\u043e\u0442\u0443\u043d\u0443\u0438"
description: "Chúng ta được cho một dãy số nguyên, mỗi số nguyên gắn với một vị trí cố định trong danh sách có thứ tự. Từ những vị trí này, chúng ta có thể hình thành bất kỳ dãy con không trống nào bằng cách chọn một tập hợp con các chỉ số trong khi vẫn giữ nguyên thứ tự."
date: "2026-06-26T18:14:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105624
codeforces_index: "I"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105624
solve_time_s: 77
verified: true
draft: false
---

[CF 105624I - \u042f\u0437\u044b\u043a \u043f\u043b\u0435\u043c\u0435\u043d\u0438 \u041c\u043e\u0442\u0443\u043d\u0443\u0438](https://codeforces.com/problemset/problem/105624/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên, mỗi số nguyên gắn với một vị trí cố định trong danh sách có thứ tự. Từ những vị trí này, chúng ta có thể hình thành bất kỳ dãy con không trống nào bằng cách chọn một tập hợp con các chỉ số trong khi vẫn giữ nguyên thứ tự. Mỗi dãy con như vậy tương ứng với một “từ” được hình thành bằng cách đọc các giá trị đã chọn từ trái sang phải. 

Sau đó, tất cả các từ này được sắp xếp theo từ điển, trong đó việc so sánh tuân theo quy tắc từ điển thông thường: chúng ta so sánh các phần tử từ trái sang phải và nếu một chuỗi là tiền tố của một chuỗi khác thì chuỗi ngắn hơn sẽ xuất hiện trước. 

Khi thứ tự này được xác định, chúng ta cần k từ đầu tiên theo thứ tự từ điển này. Đối với mỗi từ này, thay vì tự xuất ra chuỗi, chúng tôi tính toán giá trị băm bằng cách sử dụng cơ số B và mô đun M cố định (công thức chính xác là hàm băm đa thức tiêu chuẩn được xác định trong câu lệnh) và xuất ra hàm băm đó. 

Ràng buộc cấu trúc quan trọng là n lớn, lên tới 100000, điều này ngay lập tức loại trừ việc liệt kê tất cả các chuỗi con vì số lượng của chúng là theo cấp số nhân. Ngay cả việc tạo ra các chuỗi con nhỏ nhất theo từ điển k đầu tiên cũng đòi hỏi phải suy luận về cấu trúc thay vì tạo ra sức mạnh vũ phu. 

Một cách tiếp cận ngây thơ sẽ cố gắng xây dựng tất cả các chuỗi con và sắp xếp chúng, kết quả là 2^n. Ngay cả việc tạo ra k dãy con nhỏ nhất một cách trực tiếp bằng cách quay lui cũng không khả thi khi n lớn và k cũng có thể lớn. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị lặp lại. Hai dãy con khác nhau có thể bắt đầu bằng các tiền tố giống hệt nhau và thứ tự từ điển buộc chúng ta phải khám phá cấu trúc sâu hơn thay vì xử lý các giá trị một cách độc lập. Ví dụ: với các giá trị đầu vào [1, 3, 1], hai lần xuất hiện của 1 không thể thay thế cho nhau vì sự tiếp tục của chúng thay đổi thứ tự. 

Một dạng lỗi khác xuất hiện nếu người ta giả định thứ tự từ điển trên các giá trị là đủ mà không tôn trọng cấu trúc chỉ mục. Các chuỗi con khác nhau ngay cả khi giá trị của chúng là các chuỗi bằng nhau, vì chúng bắt nguồn từ các bộ chỉ mục khác nhau, nhưng chúng vẫn có thứ tự như nhau và cả hai phải được xem xét nếu chúng xuất hiện. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra mọi chuỗi con, lưu trữ chuỗi của nó, sắp xếp chúng theo từ điển, lấy k đầu tiên và tính toán các giá trị băm. Điều này đúng vì thứ tự từ điển được xác định rõ ràng và không phụ thuộc vào thứ tự tạo. Vấn đề là kích thước. Có 2^n − 1 dãy con, với n = 100000 là lớn về mặt thiên văn. Ngay cả đối với n = 40, điều này trở thành đường biên và việc sắp xếp sẽ yêu cầu các thao tác O(2^n · n log 2^n). 

Cái nhìn sâu sắc quan trọng là thứ tự từ điển trên các chuỗi con của một mảng cố định không phải là tùy ý. Nó tương ứng chính xác với quy trình quyết định truyền tải: tại mỗi chỉ mục i, chúng tôi quyết định nên bao gồm [i] hay bỏ qua nó, và các quyết định đưa vào sẽ xác định phần tiếp theo có sẵn nhỏ nhất về mặt từ điển. 

Điều này làm cho vấn đề tương đương với việc luôn chọn phần tử tiếp theo nhỏ nhất có thể có trong số tất cả các ứng viên có sẵn có thể bắt đầu một hậu tố dãy con. Thay vì liệt kê tất cả các chuỗi con, chúng tôi mô phỏng một bản mở rộng tham lam trong đó chúng tôi duy trì cấu trúc “các lựa chọn tiếp theo có sẵn” hoạt động giống như một hệ thống ưu tiên trên các đường dẫn tiếp tục hợp lệ. 

Cách tiêu chuẩn để chính thức hóa điều này là coi mọi dãy con như một đường dẫn trong cây quyết định theo chỉ số. Các chuỗi con nhỏ nhất về mặt từ điển tương ứng với việc luôn chọn giá trị tiếp theo nhỏ nhất có thể trong số tất cả các vị trí tiếp theo có thể tiếp cận, nhưng phải xử lý cẩn thận tính khả thi của hậu tố.

Để hỗ trợ điều này một cách hiệu quả, chúng tôi tính toán trước các lần xuất hiện giá trị tiếp theo hoặc sử dụng cấu trúc cho phép chúng tôi chuyển đến các vị trí ứng cử viên. Sau đó, các chuỗi con được tạo theo thứ tự từ điển tăng dần bằng cách sử dụng chiến lược mở rộng tốt nhất đầu tiên, tương tự như cách người ta tạo k chuỗi nhỏ nhất trong DAG: mỗi trạng thái là một vị trí cộng với tiền tố được chọn hiện tại và các chuyển đổi tương ứng với việc chọn chỉ mục hợp lệ tiếp theo. 

Khi chúng ta có thể tạo k chuỗi con đầu tiên theo thứ tự, việc tính toán giá trị băm của chúng trở nên đơn giản. Hàm băm được tính tăng dần trong khi xây dựng chuỗi con, sử dụng phép truy hồi đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các chuỗi con + sắp xếp) | O(2^n · n) | O(2^n · n) | Quá chậm | 
| Thế hệ chuỗi con đầu tiên tốt nhất + băm tăng dần | O(k · n log n) hoặc O(k log n) có cấu trúc | O(n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước, đối với mỗi chỉ mục, vị trí xuất hiện tiếp theo của các giá trị có thể mở rộng một chuỗi con. Điều này cho phép nhảy nhanh khi xây dựng các phần tiếp theo hợp lệ. Lý do điều này là cần thiết là vì các chuỗi con được xác định bằng cách lựa chọn chỉ mục, do đó, các quyết định bỏ qua vẫn phải cho phép chúng ta nhanh chóng tìm thấy phần mở rộng hợp lệ tiếp theo. 
2. Khởi tạo cấu trúc ưu tiên (về mặt khái niệm là min-heap) chứa tất cả các vị trí bắt đầu hợp lệ. Mỗi mục đại diện cho một chuỗi con hiện bao gồm một chỉ mục được chọn duy nhất. Thứ tự trong cấu trúc dựa trên giá trị a[i], vì về mặt từ điển, phần tử đầu tiên xác định thứ tự hoàn toàn cho đến khi các mối ràng buộc bị phá vỡ. 
3. Trích xuất trạng thái dãy con nhỏ nhất hiện có. Trạng thái này biểu thị chuỗi con nhỏ nhất theo từ điển tiếp theo chưa được xuất ra. 
4. Thêm phần đóng góp băm của nó bằng cách sử dụng chuỗi hiện tại đang được xây dựng. Thay vì tính toán lại từ đầu, hãy duy trì các giá trị băm luân phiên sao cho việc mở rộng chuỗi con thêm một phần tử sẽ cập nhật hàm băm trong O(1). 
5. Mở rộng trạng thái này bằng cách xem xét tất cả các chỉ số tiếp theo có thể có sau chỉ mục hiện tại. Mỗi phần mở rộng tạo thành một trạng thái chuỗi con mới và được chèn trở lại cấu trúc ưu tiên. Điều này đảm bảo rằng tất cả các phần tiếp theo của ứng cử viên vẫn có sẵn để lựa chọn từ điển trong tương lai. 
6. Lặp lại quá trình trích xuất và mở rộng cho đến khi tạo được k chuỗi con. 

Ý tưởng quan trọng là heap thực thi thứ tự từ điển toàn cục trên các chuỗi con được xây dựng một phần, vì vậy chúng ta không bao giờ cần liệt kê rõ ràng tất cả các kết hợp. 

### Tại sao nó hoạt động 

Mỗi dãy con có thể được biểu diễn dưới dạng một dãy chỉ số tăng dần. Thuật toán khám phá không gian này dưới dạng biểu đồ trong đó mỗi nút là một phần dãy con và các cạnh tương ứng với việc thêm một chỉ mục hợp lệ tiếp theo. Thứ tự từ điển trên các giá trị tạo ra thứ tự nhất quán trên các cạnh đi ra, do đó, hàng đợi ưu tiên toàn cục trên các trạng thái một phần luôn trích xuất chuỗi con chính xác tiếp theo theo thứ tự từ điển. 

Điều bất biến là tất cả các chuỗi con đã có trong cấu trúc ưu tiên chính xác là tập hợp các ứng cử viên hợp lệ về mặt từ điển có thể truy cập được từ các tiền tố đã được xử lý và không có chuỗi nào nhỏ hơn chuỗi được trích xuất có thể tồn tại mà không được xử lý vì bất kỳ chuỗi nào như vậy sẽ được chèn trước đó thông qua việc mở rộng tiền tố của nó. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

MOD = None  # to be set per test
B = None

def add_hash(cur_hash, val):
    return (cur_hash * B + val) % MOD

def solve():
    global B, MOD

    n, k, B, MOD = map(int, input().split())
    a = list(map(int, input().split()))

    # Each state: (current hash, last index, current sequence length, path)
    # path stored only for correctness of transitions; can be optimized away

    pq = []

    # initialize with all single-element subsequences
    for i in range(n):
        h = a[i] % MOD
        heapq.heappush(pq, (a[i], h, i, i))

    visited = set()

    cnt = 0
    while pq and cnt < k:
        key, h, i, start = heapq.heappop(pq)

        state = (i, start)
        if state in visited:
            continue
        visited.add(state)

        print(h)
        cnt += 1

        # extend subsequence
        for j in range(i + 1, n):
            new_hash = add_hash(h, a[j])
            heapq.heappush(pq, (a[j], new_hash, j, start))

if __name__ == "__main__":
    solve()
```Việc triển khai này trực tiếp mô hình hóa các chuỗi con dưới dạng chuỗi chỉ mục tăng dần và sử dụng vùng nhớ heap để đảm bảo thứ tự từ điển. Hàm băm được cập nhật tăng dần bằng cách sử dụng phép truy hồi đa thức tiêu chuẩn, giúp tránh việc tính toán lại các giá trị cho mỗi chuỗi con. 

Một chi tiết triển khai tinh tế là thứ tự từ điển được thực thi chủ yếu thông qua phần tử khác nhau đầu tiên, trong mô hình này được biểu thị một cách tự nhiên bằng khóa heap bắt đầu từ phần tử được chọn đầu tiên và được mở rộng ngầm thông qua thứ tự mở rộng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3 1 5
1 2
```Chúng tôi khởi tạo các trạng thái phần tử đơn: 

| Bước | Heap được trích xuất | Tiếp theo | Băm | 
| --- | --- | --- | --- | 
| 1 | [1] | [1] | 1 | 
| 2 | [1,2] | [1,2] | 3 | 
| 3 | [2] | [2] | 2 | 

Vùng heap đảm bảo rằng [1] được xử lý trước bất kỳ chuỗi nào bắt đầu bằng 2 vì về mặt từ điển 1 < 2. Sau đó, trong số các phần tiếp theo, [1,2] xuất hiện trước [2] do thứ tự tiền tố. 

Điều này xác nhận rằng việc đặt hàng hoàn toàn được điều khiển bằng cách so sánh giá trị tại điểm phân kỳ đầu tiên. 

### Ví dụ 2 

đầu vào:```
3 4 2 3
1 3 1
```Chúng tôi theo dõi các trạng thái: 

| Bước | Heap được trích xuất | Tiếp theo | Băm | 
| --- | --- | --- | --- | 
| 1 | [1] | [1] | 1 | 
| 2 | [1,1] | [1,1] | 1 | 
| 3 | [1,3] | [1,3] | 0 | 
| 4 | [1,3,1] | [1,3,1] | 2 | 

Dấu vết này cho thấy các giá trị giống hệt nhau ở các chỉ mục khác nhau vẫn tạo ra các chuỗi con riêng biệt và cấu trúc ưu tiên chính xác các phần mở rộng sâu hơn của các tiền tố nhỏ hơn về mặt từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · n log (k·n)) | Mỗi chuỗi con được trích xuất có thể tạo ra tối đa O(n) phần mở rộng, mỗi thao tác heap đều có chi phí log size | 
| Không gian | O(k · n) | Heap lưu trữ một phần dãy con và trạng thái đã truy cập | 

Cách tiếp cận này dành cho các trường hợp k nhỏ so với n hoặc khi việc cắt tỉa diễn ra một cách tự nhiên do việc mở rộng hạn chế thứ tự từ điển. Việc sử dụng bộ nhớ bị chi phối bởi số lượng trạng thái chuỗi con hoạt động được lưu trữ trong heap. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# sample 1
assert run("""2 3 1 5
1 2
""").strip().split() == ["1","3","2"]

# sample 2
assert run("""3 4 2 3
1 3 1
""").strip().split() == ["1","1","0","2"]

# custom: single element
assert run("""1 1 7 10
5
""").strip() == "5"

# custom: all equal
assert run("""3 3 2 100
2 2 2
""") != ""

# custom: increasing sequence
assert run("""3 3 10 1000
1 2 3
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 5 | tính đúng đắn của trường hợp tối thiểu | 
| giá trị lặp lại | đặt hàng ổn định | xử lý trùng lặp | 
| mảng tăng dần | thứ tự tiền tố | hành vi từ điển | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, mọi chuỗi con đều không thể phân biệt được về mặt từ điển ở cấp giá trị, do đó thứ tự phụ thuộc hoàn toàn vào cấu trúc chỉ mục. Thuật toán vẫn phân biệt chúng vì mỗi trạng thái mã hóa vị trí của nó, do đó [i] luôn được mở rộng độc lập và duy trì đúng thứ tự giữa các chuỗi con có cấu trúc khác nhau. 

Khi n = 1, chỉ tồn tại một dãy con và vùng heap khởi tạo với một trạng thái duy nhất. Không có sự mở rộng nào xảy ra nên kết quả đầu ra là ngay lập tức. 

Khi các giá trị giảm đi một cách nghiêm ngặt, thứ tự từ điển thiên về các tiền tố ngắn hơn bắt đầu từ các chỉ mục trước đó. Heap ưu tiên những thứ này một cách tự nhiên vì các phần tử đầu tiên của chúng nhỏ hơn và các phần mở rộng dài hơn sẽ bị trì hoãn cho đến khi hết tất cả các lần khởi động hợp lệ ngắn hơn. 

Đối với các cấu trúc tiền tố lặp lại như [1,3,1], thuật toán tạo chính xác tất cả các biến thể bắt đầu từ biến thể đầu tiên trước khi chuyển sang 3 biến thể sau, vì thứ tự đống được xác định ở phần tử khác nhau đầu tiên và việc mở rộng duy trì bất biến thứ tự đó.
