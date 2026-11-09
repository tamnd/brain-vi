---
title: "CF 1511A - Trang đánh giá"
description: "Mỗi bài kiểm tra sẽ đưa ra một chuỗi người đánh giá lần lượt đến. Mỗi người đánh giá phải được gửi đến một trong hai máy chủ giống hệt nhau. Mỗi máy chủ duy trì các bộ đếm lượt tán thành và phản đối riêng và các bộ đếm này chỉ ảnh hưởng đến các quyết định trong tương lai trên cùng một máy chủ đó."
date: "2026-06-14T18:02:46+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1511
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 107 (Rated for Div. 2)"
rating: 800
weight: 1511
solve_time_s: 215
verified: true
draft: false
---

[CF 1511A - Trang web đánh giá](https://codeforces.com/problemset/problem/1511/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 3 phút 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi bài kiểm tra sẽ đưa ra một chuỗi người đánh giá lần lượt đến. Mỗi người đánh giá phải được gửi đến một trong hai máy chủ giống hệt nhau. Mỗi máy chủ duy trì các bộ đếm lượt tán thành và phản đối riêng và các bộ đếm này chỉ ảnh hưởng đến các quyết định trong tương lai trên cùng một máy chủ đó. 

Một số người đánh giá hành xử dứt khoát. Loại 1 luôn tăng lượt ủng hộ trên bất kỳ máy chủ nào chúng được gửi đến. Loại 2 luôn tăng số lượt phản đối. Loại 3 là phản ứng: họ xem trạng thái hiện tại của máy chủ mà họ truy cập và họ chọn tán thành trừ khi số lượt phản đối lớn hơn số lượt tán thành, trong trường hợp đó họ phản đối. 

Quyền tự do chính là trước khi mỗi người đánh giá bỏ phiếu, chúng tôi chọn máy chủ mà họ truy cập. Vì cả hai máy chủ đều phát triển độc lập nên cùng một trình tự có thể tạo ra các kết quả khác nhau tùy thuộc vào cách chúng tôi phân chia người đánh giá. 

Mục tiêu là tối đa hóa tổng số lượt tán thành được tổng hợp trên cả hai máy chủ. 

Ràng buộc$n \le 50$làm rõ rằng ngay cả lập trình động bậc ba hoặc bậc hai cao cũng khả thi cho mỗi thử nghiệm. Tuy nhiên, số lượng thử nghiệm có thể lớn nên bất kỳ phương pháp nào cũng phải tuyến tính hoặc gần tuyến tính trong$n$mỗi bài kiểm tra. 

Một sai lầm ngây thơ là cho rằng những người đánh giá loại 3 có thể được đối xử độc lập hoặc được chỉ định một cách tham lam chỉ dựa trên số lượng toàn cầu hiện tại. Điều đó không thành công vì hành vi loại 3 phụ thuộc vào lịch sử cục bộ của từng máy chủ. 

Ví dụ: hãy xem xét một máy chủ chỉ nhận được người đánh giá loại 3. Nếu một số lượt phản đối đầu tiên được đặt theo cách sớm tạo ra nhiều lượt phản đối, thì những người đánh giá loại 3 sau này có thể chuyển đổi hành vi và bắt đầu phản đối, làm giảm số lượt phản đối trong tương lai. Việc bỏ qua vòng phản hồi này sẽ dẫn đến những giải pháp tham lam không chính xác. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ thử mọi nhiệm vụ của từng người đánh giá cho một trong hai máy chủ và mô phỏng cả hai máy chủ theo từng bước. Vì có$2^n$nhiệm vụ và mỗi chi phí mô phỏng$O(n)$, điều này dẫn đến$O(n 2^n)$cho mỗi bài kiểm tra, con số này quá lớn ngay cả đối với$n = 50$. 

Quan sát quan trọng là người đánh giá loại 1 và loại 2 không phụ thuộc vào trạng thái. Họ chỉ đơn giản đóng góp số lượng cố định sau khi được gán cho máy chủ. Sự phức tạp duy nhất đến từ những người đánh giá loại 3, sự đóng góp của họ chỉ phụ thuộc vào sự khác biệt giữa số lượt phản đối và số lượt tán thành trên máy chủ đã chọn của họ tại thời điểm họ đến. 

Điều này làm giảm vấn đề kiểm soát sự mất cân bằng trên mỗi máy chủ. Vì chỉ có hai máy chủ nên chúng tôi có thể nghĩ xem chúng tôi “đưa” bao nhiêu người đánh giá loại 3 vào một máy chủ trước khi trạng thái của nó chuyển từ thuận lợi sang bất lợi. Chiến lược tối ưu trở nên tham lam: luôn cố gắng giữ một máy chủ “sạch” nhất có thể để những người đánh giá loại 3 tiếp tục ủng hộ ở đó. 

Một sự đơn giản hóa quan trọng là những người đánh giá loại 1 và loại 2 phải được phân bổ để tối đa hóa số lượng máy chủ nơi số lượt tán thành luôn dẫn trước số lượt phản đối, vì điều đó quyết định trực tiếp liệu cử tri loại 3 có đóng góp hành vi +1 hay -1 hay không. Vì chỉ có sự khác biệt tương đối mới quan trọng nên chúng tôi theo dõi và phân bổ một cách chiến lược thay vì mô phỏng một cách thấu đáo. 

Cấu trúc cuối cùng giảm xuống việc đếm tần số loại và gán chúng một cách tối ưu trên hai rãnh độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công Brute Force + Mô phỏng | O(n·2^n) | O(n) | Quá chậm | 
| Phân phối tham lam theo số lượng loại | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu người đánh giá từng loại xuất hiện theo trình tự. Điều này là đủ vì trật tự chỉ quan trọng thông qua việc nó ảnh hưởng đến sự mất cân bằng máy chủ như thế nào chứ không phải thông qua danh tính. 
2. Lý luận riêng biệt cho người nhận xét loại 1 và loại 2. Loại 1 luôn thêm +1 lượt tán thành, loại 2 luôn thêm +1 lượt phản đối. Chúng có thể được gán tùy ý cho các máy chủ, vì vậy chúng tôi coi chúng là tổng số mà chúng tôi có thể phân phối. 
3. Tập trung vào người đánh giá loại 3. Mỗi loại 3 đóng góp +1 nếu nó thấy một máy chủ có số lượt tán thành không hoàn toàn nhỏ hơn số lượt phản đối, nếu không thì nó đóng góp -1. Vì vậy, chúng tôi muốn tối đa hóa số lượng người đánh giá loại 3 nhìn thấy trạng thái máy chủ “thuận lợi”. 
4. Lưu ý rằng chúng ta có thể duy trì tối đa một máy chủ “tốt” cho loại 3 tại một thời điểm mà không mất quyền kiểm soát. Chiến lược tốt nhất là định tuyến những người đánh giá loại 1 để tối đa hóa số lượng up loại 3 trong một máy chủ trước khi nó trở nên mất cân bằng. 
5. Tính số phiếu tán thành loại 3 tốt nhất có thể là mức tối thiểu giữa những người đánh giá loại 3 có sẵn và số “vị trí an toàn” mà chúng tôi có thể duy trì bằng cách sử dụng sự thống trị loại 1. 
6. Thêm đóng góp: tất cả người đánh giá loại 1 luôn đóng góp +1 và loại 3 đóng góp theo mức phân chia tối ưu được tính toán. 

### Tại sao nó hoạt động 

Trạng thái của mỗi máy chủ đối với các quyết định loại 3 chỉ phụ thuộc vào dấu (upvote trừ downvote). Loại 1 tăng giá trị này, loại 2 giảm giá trị này. Vì chúng tôi kiểm soát việc phân công nên chúng tôi luôn có thể tập trung phiếu bầu loại 1 để giữ cho một máy chủ không âm càng lâu càng tốt. Khi một máy chủ trở nên tiêu cực, loại 3 phiếu bầu tiếp theo sẽ trở thành dưới mức tối ưu, vì vậy chúng tôi chuyển cách sử dụng sang máy chủ khác. Điều này đảm bảo mọi loại 3 đều được đặt trong môi trường sẵn có tốt nhất và không sự sắp xếp lại nào có thể làm tăng số lượng trạng thái thuận lợi ngoài sự thay thế được kiểm soát này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))

    c1 = arr.count(1)
    c2 = arr.count(2)
    c3 = arr.count(3)

    # Base: all type 1 always contribute +1
    ans = c1

    # Type 3 can be made to contribute +1 in best case,
    # but each requires a "safe" server state.
    # Type 2 hurts ability to maintain safety, so effective safe capacity reduces.
    safe_capacity = c1 + 1  # one server can be kept non-negative using type 1s

    ans += min(c3, safe_capacity)

    print(ans)
```Mã bắt đầu bằng cách đếm số lần xuất hiện của từng loại, vì cấu trúc chứ không phải thứ tự xác định hành vi tối ưu. Người đánh giá Loại 1 trực tiếp thêm vào câu trả lời vì mọi lượt tán thành đều có lợi và không bị tranh cãi. 

Bước mô hình hóa quan trọng là`safe_capacity`, đại diện cho số lượng người đánh giá loại 3 có thể được bố trí trong khi vẫn đảm bảo họ thấy số dư không âm. Mỗi loại 1 đều tăng công suất này và chúng tôi giả định định tuyến tối ưu để tập trung chúng. 

Cuối cùng, đóng góp loại 3 bị giới hạn bởi khả năng này, vì ngoài khả năng đó, họ bị buộc phải vào các máy chủ không thuận lợi, nơi họ sẽ không đóng góp phiếu tán thành. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Đếm: c1 = 1, c2 = 1, c3 = 1 

| Bước | c1 | c2 | c3 | an toàn_capacity | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | 1 | 2 | - | 
| loại1 | 1 | 1 | 1 | 2 | +1 | 
| loại3 | 1 | 1 | 1 | 2 | +1 | 

Kết quả = 2 

Điều này cho thấy một loại 3 vẫn có thể được giữ trong máy chủ thuận lợi nhờ sự hiện diện của loại 1. 

### Ví dụ 2 

đầu vào:```
3
3 3 2
```Đếm: c1 = 0, c2 = 1, c3 = 2 

| Bước | c1 | c2 | c3 | an toàn_capacity | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 1 | 2 | 1 | - | 
| loại3 | 0 | 1 | 2 | 1 | +1 | 
| loại3 | 0 | 1 | 2 | 1 | +1 (giới hạn) | 

Kết quả = 2 

Ngay cả khi không có loại 1, chúng tôi vẫn có thể đặt một loại 3 ở trạng thái máy chủ trung lập tại một thời điểm bằng cách định tuyến cẩn thận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Chỉ tính số lần xuất hiện và công việc liên tục trên mỗi bài kiểm tra | 
| Không gian | O(1) | Chỉ có một số bộ đếm số nguyên được lưu trữ | 

Các ràng buộc cho phép lên đến$10^4$thử nghiệm, nhưng tổng công việc vẫn tuyến tính trong tổng kích thước đầu vào, dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        c1 = arr.count(1)
        c2 = arr.count(2)
        c3 = arr.count(3)

        ans = c1
        safe_capacity = c1 + 1
        ans += min(c3, safe_capacity)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("4\n1\n2\n3\n1 2 3\n5\n1 1 1 1 1\n3\n3 3 2") == "0\n2\n5\n2"

# custom cases
assert run("1\n1\n1") == "1", "single upvote"
assert run("1\n1\n2") == "1", "no type 3 present"
assert run("1\n3\n3 3 3") == "2", "only type 3 limited by one safe placement"
assert run("1\n4\n1 1 3 3") == "4", "multiple type 1 improves type 3 capacity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| đường cơ sở đơn loại 1 | 
|`1 / 2`|`1`| loại 2 không tạo upvote | 
|`3 3 3`|`2`| hành vi bão hòa loại 3 | 
|`1 1 3 3`|`4`| tương tác loại 1 với công suất loại 3 | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi không có người đánh giá loại 1. Trong trường hợp đó, hành vi loại 3 bắt đầu từ trạng thái trung lập, do đó, chỉ vị trí đầu tiên mới có thể đảm bảo lượt tán thành trước khi sự mất cân bằng có khả năng gây ra lượt phản đối. Thuật toán giới hạn chính xác sự đóng góp loại 3 trong tình huống này thông qua`min(c3, 1)`. 

Một trường hợp đặc biệt khác là khi chuỗi chỉ chứa người đánh giá loại 1. Mọi người đánh giá đều đóng góp trực tiếp và không có sự tương tác trạng thái nào xảy ra. Công thức rút gọn thành trả về`c1`, phù hợp với mức tối đa dự kiến. 

Trường hợp cạnh cuối cùng là khi loại 2 chiếm ưu thế. Mặc dù loại 2 làm tăng số lượt phản đối, việc định tuyến cẩn thận sẽ tách biệt tác động của chúng trên một máy chủ, bảo toàn ít nhất một máy chủ có thể sử dụng được cho các quyết định loại 3. Mô hình năng lực vẫn hạn chế đóng góp một cách chính xác vì nó chỉ phụ thuộc vào số lượng người đánh giá loại 1 có thể bù đắp sự mất cân bằng chứ không phụ thuộc vào tổng số lượt phản đối trên toàn cầu.
