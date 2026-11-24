---
title: "CF 899A - Chia nhóm"
description: "Chúng tôi được giao cho một số nhóm sinh viên. Mỗi nhóm có một hoặc hai học sinh và các nhóm này không thể chia cắt được. Huấn luyện viên muốn thành lập càng nhiều đội càng tốt, trong đó mỗi đội phải có đúng ba học sinh."
date: "2026-06-17T03:27:07+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 899
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 452 (Div. 2)"
rating: 800
weight: 899
solve_time_s: 64
verified: true
draft: false
---

[CF 899A - Chia nhóm](https://codeforces.com/problemset/problem/899/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao cho một số nhóm sinh viên. Mỗi nhóm có một hoặc hai học sinh và các nhóm này không thể chia cắt được. Huấn luyện viên muốn thành lập càng nhiều đội càng tốt, trong đó mỗi đội phải có đúng ba học sinh. 

Một nhóm quy mô một đóng góp một học sinh linh hoạt. Một nhóm cỡ hai đóng góp một cặp cố định mà cả hai phải được sử dụng trong cùng một đội hoặc hoàn toàn không được sử dụng. Chúng tôi không thể chia một cặp thành các đội khác nhau hoặc chỉ kết hợp một thành viên của cặp với người khác. 

Nhiệm vụ là chọn một số nhóm và phân học sinh của họ thành các nhóm riêng biệt có quy mô ba, tối đa hóa số lượng nhóm hoàn chỉnh. 

Ràng buộc n có thể lớn tới 200.000, loại trừ mọi cách tiếp cận thử tất cả các kết hợp nhóm hoặc xây dựng nhóm tăng dần bằng cách quay lui. Bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính về số lượng nhóm. 

Một trường hợp thất bại tinh tế xuất hiện khi trực giác tham lam được áp dụng không đúng cách. Ví dụ: nếu chúng ta luôn cố gắng thành lập một đội ngay lập tức bất cứ khi nào chúng ta thấy đủ học sinh độc thân, chúng ta có thể bỏ lỡ cơ hội kết hợp một cặp với hai đơn sau đó. 

Đầu vào như:```
5
1 1 1 2 2
```Một kẻ tham lam ngây thơ có thể thành lập một đội từ ba người và sau đó mắc kẹt với hai cặp không thể tạo thành một đội đầy đủ, cho kết quả là 1. Tuy nhiên, lý luận tối ưu sẽ tạo ra 2 đội bằng cách sử dụng cả hai cặp với các học sinh đơn lẻ một cách thích hợp. 

Điều này cho thấy vấn đề không nằm ở việc phân nhóm cục bộ mà là ở việc đếm tài nguyên trên toàn cầu. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ cố gắng chọn các tập hợp con của các nhóm và gán chúng thành các nhóm ba người trong khi tôn trọng ràng buộc rằng mỗi nhóm kích thước hai được chọn sẽ được bao gồm hoặc loại trừ hoàn toàn. Điều này trở thành vấn đề phân vùng trên các mục có trọng số trong đó mỗi mục là 1 hoặc 2 và chúng tôi thử tất cả các kết hợp chọn hoặc bỏ qua từng nhóm. 

Đối với mỗi tập hợp con của các nhóm được chọn, chúng tôi sẽ cố gắng đóng gói tổng kích thước của chúng thành các nhóm ba, có thể sử dụng phương pháp quay lui hoặc gán động. Chỉ riêng số lượng tập hợp con là 2^n và thậm chí việc kiểm tra tính khả thi trên mỗi tập hợp con sẽ tốn ít nhất thời gian tuyến tính, khiến điều này hoàn toàn không khả thi ở mức n = 200.000. 

Quan sát quan trọng là chỉ tính vật chất chứ không tính đến danh tính của các nhóm. Gọi x là số nhóm đơn và y là số nhóm đôi. Chúng tôi đang cố gắng phân chia một cách hiệu quả một tập hợp gồm x đơn vị và y two thành ba phần, với hạn chế là không thể chia hai phần. 

Cấu trúc đơn giản hóa vì bất kỳ đội hợp lệ nào cũng là một trong hai loại hữu ích: ba đơn hoặc một đôi cộng một đơn. Một đội gồm ba người là không thể vì nó đòi hỏi phải chia đôi. Điều này làm giảm vấn đề phân phối đĩa đơn xung quanh các cặp cố định. 

Chiến lược tham lam xuất phát từ thực tế rằng các cặp đôi là nguồn tài nguyên đắt tiền: chúng tiêu thụ hai người cùng một lúc và kém linh hoạt hơn. Cấu trúc tối ưu luôn sử dụng các cặp bằng cách kết hợp chúng với các cặp đơn bất cứ khi nào có thể, bởi vì để lại một cặp không được sử dụng sẽ lãng phí hai đóng góp tiềm năng khó có thể tái sử dụng ở nơi khác. 

Do đó, chúng tôi ưu tiên thành lập các nhóm sử dụng một cặp và một chiếc cho đến khi hết một trong hai. Sau đó, những người đánh đơn còn lại tạo thành đội ba người. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén dữ liệu đầu vào thành hai số: số lượng nhóm một người và số lượng nhóm hai người. 

1. Đếm xem có bao nhiêu nhóm cỡ 1 và có bao nhiêu nhóm cỡ 2. Điều này làm giảm vấn đề xuống còn hai số nguyên thay vì một mảng. 
2. Đầu tiên, cố gắng sử dụng từng nhóm cỡ 2 cùng với nhóm cỡ 1 để tạo thành một nhóm đầy đủ gồm ba người. Mỗi cặp như vậy tiêu thụ một trong mỗi loại và tạo ra chính xác một đội. 
3. Số lượng các đội như vậy bị giới hạn bởi số lượng nhỏ hơn trong hai đội, vì mỗi đội cần cả một đôi và một đội. 
4. Sau bước này, cập nhật số đếm còn lại bằng cách trừ đi số đơn và số cặp đã sử dụng. 
5. Bây giờ chỉ có những người độc thân vẫn còn phù hợp. Mỗi đội yêu cầu chính xác ba đĩa đơn, vì vậy hãy chia số lượng đĩa đơn còn lại cho ba để có thêm đội. 
6. Tổng hợp cả hai đóng góp để có được câu trả lời cuối cùng. 

Lý do đằng sau việc ưu tiên các đội đơn theo cặp là vì một cặp không thể đóng góp vào bất kỳ cấu hình nào khác ngoại trừ việc được ghép với chính xác một đội đơn hoặc không được sử dụng. Vì việc không sử dụng một cặp còn tệ hơn nhiều so với việc kết hợp nó với một cặp bất cứ khi nào có thể, nên bước so khớp này luôn an toàn. 

Tại sao nó hoạt động: mọi giải pháp hợp lệ đều có thể được chuyển đổi thành một giải pháp trong đó tất cả các đội theo cặp cộng với một đội có thể được thành lập trước mà không làm giảm tổng số đội. Bất kỳ sai lệch nào cũng sẽ lãng phí một cặp hoặc sử dụng nhiều đơn vị hơn mức cần thiết trong các cấu hình có thể được sắp xếp lại thành số lượng bộ ba bằng nhau hoặc tốt hơn. Điều này chứng tỏ rằng việc ghép đôi tham lam sau đó đóng gói các đĩa đơn còn lại là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

ones = 0
twos = 0

for x in a:
    if x == 1:
        ones += 1
    else:
        twos += 1

# pair each 2-group with a 1-group if possible
pair_teams = min(ones, twos)
ones -= pair_teams
twos -= pair_teams

# remaining ones form teams of 3
single_teams = ones // 3

print(pair_teams + single_teams)
```Mã bắt đầu bằng cách nén đầu vào thành hai bộ đếm, đây là sự đơn giản hóa trung tâm. Điều này tránh việc theo dõi các nhóm riêng lẻ và chỉ tập trung vào các nguồn lực sẵn có. 

Giai đoạn đầu tiên tính toán có thể hình thành bao nhiêu (2,1) kết hợp. sử dụng`min(ones, twos)`đảm bảo chúng tôi không bao giờ lạm dụng tài nguyên. Sau khi trừ đi, chúng tôi phản ánh chính xác những gì còn lại để phân nhóm thêm. 

Cuối cùng, những cái còn lại được nhóm một cách tham lam thành bộ ba, vì không còn cấu trúc nào khác có thể liên quan đến chúng. 

Một lỗi phổ biến ở đây là cố gắng tạo thành bộ ba số một trước khi ghép đôi với số hai. Điều đó dẫn đến việc hai người bị mắc kẹt không thể tham gia vào bất kỳ đội nào, làm giảm tổng số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1 2 1
```Đếm: 

một = 3, hai = 1 

| Bước | những cái | hai người | cặp_đội | những cái còn lại | hai phần còn lại | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 3 | 1 | 0 | - | - | 
| hình thành cặp | 2 | 0 | 1 | 2 | 0 | 
| đội hình ba | 2 | 0 | 1 | 0 | 0 | 

Kết quả cuối cùng là 1 đội. 

Điều này cho thấy rằng mặc dù ban đầu có ba đĩa đơn, nhưng một trong số chúng được sử dụng tốt hơn để mở khóa cặp, vì chỉ riêng cặp đó không thể tạo thành một đội. 

### Ví dụ 2 

đầu vào:```
6
1 1 1 1 2 2
```Đếm: 

một = 4, hai = 2 

| Bước | những cái | hai người | cặp_đội | những cái còn lại | hai phần còn lại | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 4 | 2 | 0 | - | - | 
| hình thành cặp | 2 | 0 | 2 | 2 | 0 | 
| đội hình ba | 2 | 0 | 2 | 0 | 0 | 

Kết quả cuối cùng là 2 đội. 

Điều này chứng tỏ rằng tất cả các cặp được sử dụng tốt nhất ngay lập tức với các cặp đơn, tối đa hóa việc sử dụng trước khi hình thành các bộ ba còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần tới các nhóm đếm, số học theo thời gian không đổi sau đó | 
| Không gian | O(1) | Chỉ các bộ đếm cho một và hai được lưu trữ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì ngay cả đối với 200.000 nhóm, chúng tôi chỉ thực hiện quét tuyến tính và tính toán theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    ones = 0
    twos = 0

    for x in a:
        if x == 1:
            ones += 1
        else:
            twos += 1

    pair_teams = min(ones, twos)
    ones -= pair_teams
    twos -= pair_teams

    single_teams = ones // 3

    return str(pair_teams + single_teams)

# provided sample
assert run("4\n1 1 2 1\n") == "1"

# all ones, no pairs
assert run("3\n1 1 1\n") == "1"

# all twos, cannot form any team
assert run("2\n2 2\n") == "0"

# mixed, but singles insufficient
assert run("5\n2 2 1 1 1\n") == "2"

# large balanced case
assert run("6\n1 1 1 1 2 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | 1 | hình thành bộ ba cơ bản | 
| tất cả hai | 0 | từng cặp không thể tạo thành đội | 
| trộn nhỏ | 2 | tham lam ghép nối đúng đắn | 
| cân bằng | 2 | sử dụng đầy đủ các cặp | 

## Vỏ cạnh 

Trường hợp một bên là khi không có nhóm người nào cả. 

đầu vào:```
4
2 2 2 2
```Ở đây những cái = 0, hai cái = 4. Bước ghép đôi tạo ra 0 đội vì không tồn tại một nhóm nào. Các số đơn còn lại vẫn bằng 0, vì vậy câu trả lời là 0. Thuật toán tránh cố gắng ép buộc sử dụng cặp không hợp lệ một cách chính xác. 

Một trường hợp khác là khi có nhiều cá thể đơn lẻ nhưng lại vắng mặt các cặp cá thể. 

đầu vào:```
6
1 1 1 1 1 1
```Ở đây những cái = 6, hai cái = 0. Bước ghép đôi tạo ra 0 đội. Những người độc thân còn lại tạo thành 2 đội thông qua 6 // 3. Điều này xác nhận rằng thuật toán không phụ thuộc vào sự hiện diện của các cặp và đơn giản hóa thành việc nhóm đơn giản. 

Trường hợp cạnh thứ ba là khi số lượng đơn và cặp bằng nhau, giúp tối đa hóa sự tương tác giữa hai loại. 

đầu vào:```
5
1 1 1 2 2
```một = 3, hai = 2. Ghép đôi sẽ tạo ra 2 đội, chỉ còn lại một đội không được sử dụng. Không thể tạo thêm bộ ba nào nữa và thuật toán đưa ra kết quả chính xác là 2. Điều này chứng tỏ rằng các đơn vị còn sót lại dưới ba sẽ bị loại bỏ một cách tự nhiên mà không cần xử lý đặc biệt.
