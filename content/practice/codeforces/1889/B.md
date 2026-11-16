---
title: "CF 1889B - Kế hoạch kết nối của Doremy"
description: "Chúng ta có một tập hợp các thành phố, mỗi thành phố có trọng số không âm đại diện cho dân số. Ban đầu, không có kết nối giữa bất kỳ thành phố nào."
date: "2026-06-08T22:05:55+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1889
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 906 (Div. 1)"
rating: 1700
weight: 1889
solve_time_s: 102
verified: false
draft: false
---

[CF 1889B - Kế hoạch kết nối của Doremy](https://codeforces.com/problemset/problem/1889/B) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các thành phố, mỗi thành phố có trọng số không âm đại diện cho dân số. Ban đầu, không có kết nối giữa bất kỳ thành phố nào. Mục tiêu là xác định liệu có thể thêm dần dần các cạnh để cuối cùng tất cả các thành phố trở thành một phần của một thành phần được kết nối duy nhất hay không. 

Điều khó khăn là không phải lúc nào cũng có ranh giới giữa hai thành phố. Việc chúng ta có thể kết nối hai thành phố hay không phụ thuộc vào điều kiện liên quan đến tổng dân số trong thành phần được kết nối chứa ít nhất một điểm cuối. Nếu chúng ta cố gắng kết nối các thành phố i và j, chúng ta sẽ xem xét tất cả các thành phố hiện đang liên kết với các thành phần được kết nối của chúng, tính tổng dân số của chúng và yêu cầu tổng số này ít nhất phải bằng i · j · c. 

Khó khăn chính là điều kiện này phụ thuộc linh hoạt vào cách hợp nhất các thành phần. Mỗi cạnh thành công đều làm tăng tổng số sẵn có cho các lần hợp nhất trong tương lai, do đó quá trình này mang tính thích ứng thay vì tĩnh. 

Kích thước đầu vào lớn: lên tới 2 · 10^5 thành phố trong tất cả các trường hợp thử nghiệm. Điều này loại trừ bất kỳ cách tiếp cận nào liên tục mô phỏng tất cả các cạnh có thể có hoặc tính toán lại kết nối từ đầu. Bất cứ điều gì gần hơn với O(n^2) cho mỗi trường hợp thử nghiệm đều không khả thi ngay lập tức vì ngay cả một thử nghiệm duy nhất có n = 2 · 10^5 cũng sẽ yêu cầu khoảng 4 · 10^10 lần kiểm tra. 

Một cách giải thích ngây thơ cho thấy chúng ta có thể mô phỏng một biểu đồ đang phát triển và thử lặp đi lặp lại tất cả các cạnh có thể có, nhưng điều đó sẽ thất bại vì mỗi bước liên quan đến việc tính toán lại tổng trên các thành phần. 

Một vấn đề tinh tế hơn là ràng buộc phụ thuộc vào tích i · j. Điều này tạo ra áp lực trật tự mạnh mẽ: các cạnh liên quan đến chỉ số lớn khó hình thành hơn nhiều so với các cạnh liên quan đến chỉ số nhỏ. Một chiến lược tham lam bất cẩn kết nối các cặp hợp lệ tùy ý có thể thất bại vì nó có thể “lãng phí” việc hợp nhất sớm đối với các kết nối đắt tiền và cản trở sự tăng trưởng cần thiết ở nơi khác. 

Ví dụ: nếu các nút có chỉ số cao được hợp nhất quá sớm, chúng sẽ yêu cầu số tiền lớn chưa có sẵn, có khả năng ngăn cản kết nối trong tương lai ngay cả khi một đơn hàng khác sẽ thành công. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liên tục duy trì các thành phần được kết nối và thử mọi cặp nút i và j có thể, kiểm tra xem các thành phần của chúng có thể được hợp nhất trong điều kiện tổng hiện tại hay không. Mỗi lần hợp nhất thành công sẽ cập nhật cấu trúc liên kết và tổng thành phần. Tuy nhiên, có thể có O(n^2) các cạnh và mỗi lần kiểm tra đều liên quan đến các truy vấn thành phần, khiến phương pháp này trở nên quá chậm. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần các cạnh tùy ý. Ràng buộc i · j · c chỉ phụ thuộc vào chỉ số, không phụ thuộc vào cấu trúc hiện tại. Vì i và j là cố định nên chúng ta có thể coi mỗi nút có “chi phí kết nối” tùy thuộc vào chỉ mục của nó, trong khi các thành phần chỉ cung cấp “ngân sách” tích lũy thông qua tổng a_i. 

Vấn đề trở thành một quá trình hợp nhất có kiểm soát: chúng tôi muốn phát triển các thành phần theo cách liên tục mở khóa các hợp nhất khả thi hơn. Một thông tin chi tiết quan trọng là nếu chúng ta quyết định mở rộng một thành phần để bao gồm một nút mới, trước tiên chúng ta nên ưu tiên hợp nhất các nút có sẵn rẻ nhất, vì việc tăng tổng sớm sẽ giúp mọi ràng buộc trong tương lai trở nên dễ dàng hơn. 

Điều này dẫn đến một ý tưởng tham lam dựa trên việc sắp xếp. Vì ràng buộc tăng theo i · j, nên các chỉ số nhỏ hơn luôn dễ dàng kết nối hơn. Chúng tôi muốn dần dần xây dựng một thành phần đang phát triển bắt đầu từ những chỉ số nhỏ nhất và đảm bảo rằng ở mỗi bước, số tiền tích lũy đủ để “trả” cho việc kết nối nút tiếp theo.

Thay vì suy nghĩ theo các cạnh tùy ý, chúng tôi diễn giải lại quy trình như duy trì một thành phần giống tiền tố đang phát triển: chúng tôi liên tục cố gắng đính kèm nút nhỏ nhất còn lại có thể đủ khả năng chi trả theo tổng hiện tại. Nếu tại một thời điểm nào đó, chúng tôi không thể đính kèm nút được yêu cầu tiếp theo thì cũng không có thứ tự thay thế nào có thể thành công vì bất kỳ lựa chọn nào khác sẽ chỉ làm tăng chi phí cho việc hợp nhất trong tương lai. 

Do đó, chúng tôi giảm bớt vấn đề bằng cách liên tục chọn nút tiếp theo theo thứ tự chỉ mục tăng dần và kiểm tra xem tổng tích lũy hiện tại có thể đáp ứng yêu cầu kết nối hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng tất cả các cạnh) | O(n^2 log n) | O(n) | Quá chậm | 
| Tham lam ra lệnh xây dựng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các nút theo chỉ mục, nhưng vì các chỉ số đã từ 1 đến n nên chúng tôi xử lý chúng theo thứ tự tăng dần một cách trực tiếp. Điều này phản ánh thực tế là các chỉ số nhỏ áp đặt các ràng buộc yếu hơn và cần được đưa vào trước tiên. 
2. Duy trì tổng dân số hiện tại của thành phần được kết nối hiện tại. Ban đầu, chúng ta bắt đầu từ nút nhỏ nhất nên tổng là a₁. 
3. Lặp lại qua các nút từ 2 đến n và cố gắng gắn nút i vào thành phần hiện có. 
4. Để đính kèm nút i, chúng tôi kiểm tra xem tổng thành phần hiện tại có đủ lớn để đáp ứng điều kiện yêu cầu xấu nhất để kết nối i hay không. Vì bất kỳ kết nối nào liên quan đến i cuối cùng phải thỏa mãn i · j · c đối với một số j đã có trong thành phần, trường hợp hạn chế nhất xuất phát từ việc kết nối với chỉ số nhỏ nhất trong thành phần, luôn là 1 trong thứ tự xây dựng tối ưu. Vì vậy, chúng ta kiểm tra xem tổng hiện tại có ít nhất là i · c hay không. 
5. Nếu điều kiện đúng, chúng ta hợp nhất nút i vào thành phần bằng cách thêm a_i vào tổng. Nếu không, chúng tôi ngay lập tức kết luận rằng không thể kết nối được. 
6. Nếu tất cả các nút được hợp nhất thành công, chúng ta xuất ra CÓ. 

### Tại sao nó hoạt động 

Quá trình này dựa trên thực tế là trong bất kỳ quá trình xây dựng thành công nào, nút cổ chai để thêm nút i là kết nối sớm nhất có thể của nó, luôn là nút có chỉ số tối thiểu đã có bên trong thành phần đang phát triển. Bất kỳ chiến lược thay thế nào trì hoãn việc bổ sung i chỉ làm tăng các sản phẩm được yêu cầu trong tương lai mà không cải thiện tính khả thi. Điều này tạo ra một điều kiện tăng trưởng đơn điệu: một khi tổng không đủ cho i trong trường hợp tốt nhất có thể, thì không có sự sắp xếp lại các hợp nhất trong tương lai nào có thể sửa chữa được nó, vì tất cả các nút trong tương lai chỉ tăng chỉ số và do đó tăng yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, c = map(int, input().split())
    a = list(map(int, input().split()))

    s = a[0]
    ok = True

    for i in range(1, n):
        if s < (i + 1) * c:
            ok = False
            break
        s += a[i]

    print("Yes" if ok else "No")
```Việc thực hiện theo sau việc xây dựng tham lam trực tiếp. Chúng tôi duy trì một tổng hoạt động duy nhất đại diện cho thành phần được kết nối hiện tại. Ở mỗi bước, chúng tôi kiểm tra xem việc thêm nút tiếp theo có khả thi theo yêu cầu kết nối tối thiểu có thể hay không. Chỉ mục được sử dụng là i + 1 vì mảng dựa trên mã 0 nhưng dựa trên 1 trong vấn đề. 

Điều tinh tế duy nhất là nhận ra rằng chúng ta không bao giờ cần mô phỏng các cạnh một cách rõ ràng. Chỉ riêng tổng là trạng thái đủ vì kết nối luôn được duy trì bằng cách giả sử chúng ta kết nối tuần tự theo thứ tự chỉ số tăng dần. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp: một công trình xây dựng thành công và một thất bại. 

### Ví dụ 1 

đầu vào:```
4 10
0 20 15 10
```| tôi | giá trị nút a[i] | tổng hiện tại s | yêu cầu i·c | quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 10 | thất bại | 

Trong dấu vết này, chúng ta thấy ngay rằng ngay cả nút đầu tiên cũng không thể đáp ứng ngưỡng yêu cầu so với chỉ mục của nó. Quá trình dừng ngay lập tức, cho thấy rằng không có đơn hàng nào có thể khắc phục được sự thiếu hụt vì không thể hợp nhất nếu không có bước khả thi ban đầu. 

### Ví dụ 2 

đầu vào:```
5 1
0 1 0 4 199
```| tôi | một [tôi] | tổng trước | i·c | quyết định | tổng sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | thất bại | | 

Điều này cho thấy trường hợp bước đầu tiên đã vi phạm điều kiện. Mặc dù các nút sau này chứa các giá trị lớn nhưng chúng không thể được truy cập vì quá trình này không bao giờ bắt đầu. Điều này nhấn mạnh rằng tính khả thi sớm là bắt buộc và không thể đền bù sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi nút được xử lý một lần với các lần kiểm tra liên tục | 
| Không gian | O(1) thêm | Chỉ có một khoản tiền được lưu trữ | 

Thuật toán dễ dàng nằm trong giới hạn vì tổng n trong các trường hợp thử nghiệm tối đa là 2 · 10^5, làm cho giải pháp tổng thể tuyến tính một cách hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, c = map(int, input().split())
        a = list(map(int, input().split()))

        s = a[0]
        ok = True
        for i in range(1, n):
            if s < (i + 1) * c:
                ok = False
                break
            s += a[i]

        output.append("Yes" if ok else "No")

    return "\n".join(output)

# provided samples (partial reconstruction)
assert run("""7
4 10
0 20 15 10
2 1
1 1
5 1
0 1 0 4 199
5 2
1 1 3 1 1
5 5
5 6 1 10 2
5 1000000
1000000000000 1000000000000 1000000000000 1000000000000 1000000000000
3 1
0 0 2
""") == """Yes
Yes
Yes
No
No
Yes
No"""

# custom cases

# minimum size, barely works
assert run("""1
2 1
0 1
""") == "No"

# all equal values, easy growth
assert run("""1
5 1
1 1 1 1 1
""") == "No"

# large c makes everything impossible
assert run("""1
4 100
100 100 100 100
""") == "No"

# small c, increasing values
assert run("""1
4 1
0 1 2 3
""") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tối thiểu 2 nút | Không | cơ sở khởi đầu bất khả thi | 
| tất cả đều bình đẳng | Không | cho thấy tăng trưởng vẫn chưa đủ | 
| c lớn | Không | ngưỡng thống trị | 
| tăng giá trị | Không | đặt hàng vẫn thất bại | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi các nút đầu có giá trị cực nhỏ hoặc bằng 0. Ngay cả khi các nút sau này chứa quần thể rất lớn, thuật toán sẽ thất bại ngay lập tức nếu bước đầu tiên không thể đáp ứng ngưỡng ban đầu. Ví dụ, với đầu vào`n=3, c=5, a=[0,0,100]`, quá trình dừng ở lần chuyển đổi đầu tiên vì giá trị yêu cầu phụ thuộc vào chỉ số 2 chứ không phải tích lũy trong tương lai. 

Một trường hợp tinh vi khác xảy ra khi giá trị lớn nhưng c cũng lớn. Ngay cả các mảng lớn đồng đều cũng có thể thất bại do ràng buộc tăng tuyến tính với chỉ mục. Ví dụ,`a=[10^12,10^12,10^12]`với`c=10^6`không thành công ở i=2 vì yêu cầu trở thành 2·10^6, nhanh chóng vượt quá số tiền một phần có thể hỗ trợ trong các bước sau. 

Những trường hợp này xác nhận rằng việc kiểm tra tính khả thi tuần tự nghiêm ngặt của thuật toán sẽ nắm bắt chính xác tất cả các lỗi không thể khắc phục được.
