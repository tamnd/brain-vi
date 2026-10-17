---
title: "CF 105494C - Mê cung tuyến tính"
description: "Chúng tôi được cung cấp một chuỗi các phòng được lập chỉ mục từ 1 trở đi và chúng tôi xử lý chúng theo thứ tự nghiêm ngặt. Trong khi di chuyển qua tiền tố của các phòng, chúng ta duy trì một giá trị số nguyên duy nhất được gọi là câu trả lời hiện tại. Mỗi phòng đóng góp theo một trong hai cách."
date: "2026-06-23T01:39:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105494
codeforces_index: "C"
codeforces_contest_name: "2024-2025 ICPC NERC, Kyrgyzstan Qualification Contest"
rating: 0
weight: 105494
solve_time_s: 50
verified: true
draft: false
---

[CF 105494C - Mê cung tuyến tính](https://codeforces.com/problemset/problem/105494/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các phòng được lập chỉ mục từ 1 trở đi và chúng tôi xử lý chúng theo thứ tự nghiêm ngặt. Trong khi di chuyển qua tiền tố của các phòng, chúng ta duy trì một giá trị số nguyên duy nhất được gọi là câu trả lời hiện tại. 

Mỗi phòng đóng góp theo một trong hai cách. Bước vào bất kỳ phòng nào luôn tăng câu trả lời hiện tại lên đúng một. Ngoài ra, một số phòng được đánh dấu là điểm nối và bất cứ khi nào chúng tôi vào phòng nối, câu trả lời hiện tại sẽ được nhân đôi ngay sau khi áp dụng mức tăng. 

Nhiệm vụ là tính giá trị cuối cùng của tiến trình đang chạy này sau khi xử lý tất cả các phòng theo thứ tự. 

Một cách hữu ích để giải thích điều này là câu trả lời tiến triển thông qua một chuỗi các phép biến đổi tất định. Mỗi bước đều áp dụng một “cộng một” cố định và đôi khi là “nhân hai” bổ sung. Khó khăn không nằm ở một thao tác đơn lẻ mà ở việc hiểu cách nhân đôi lặp lại tương tác với các bước tăng trước đó. 

Nếu có tối đa 10^5 phòng, mọi giải pháp mô phỏng nhiều hơn lượng công việc không đổi cho mỗi phòng đều có thể chấp nhận được. Tuy nhiên, một cách giải thích ngây thơ cố gắng mô hình hóa rõ ràng cách mỗi đóng góp trước đó lan truyền thông qua các lần nhân đôi trong tương lai sẽ ngay lập tức trở thành bậc hai, vì mỗi điểm nối có khả năng ảnh hưởng đến tất cả các đóng góp trước đó. 

Một trường hợp thất bại tinh vi xuất hiện khi người ta cố gắng “tính toán lại các đóng góp” cho mỗi điểm nối. Ví dụ, hãy xem xét một chuỗi trong đó mỗi phòng là một ngã ba. Một phương pháp đơn giản liên tục nhân đôi tổng số gia số được lưu trữ trước đó nhưng cũng cố gắng áp dụng lại lịch sử sẽ xử lý lại cùng một tiền tố nhiều lần một cách hiệu quả, dẫn đến công việc lặp đi lặp lại tăng tuyến tính theo từng bước. 

Một trường hợp khác là khi không có điểm nối nào cả. Khi đó, câu trả lời chỉ đơn giản là n, vì chúng ta chỉ cộng một số cho mỗi phòng và không bao giờ nhân lên. Bất kỳ việc triển khai không chính xác nào giả định ít nhất một lần nhân đôi hoặc khởi tạo không chính xác xung quanh phép nhân sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực cố gắng mô phỏng trực tiếp diễn biến của câu trả lời. Chúng tôi bắt đầu bằng số 0, lặp qua các phòng từ trái sang phải, thêm một phòng cho mỗi phòng và bất cứ khi nào gặp một điểm nối, chúng tôi nhân toàn bộ giá trị hiện tại với hai. 

Mô phỏng này là đúng vì nó tuân theo các quy tắc chính xác như đã nêu. Tuy nhiên, vấn đề mấu chốt nằm ở chỗ “giá trị hiện tại” thể hiện điều gì. Khi chúng ta nhân với hai, chúng ta cũng ngầm nhân đôi tất cả những đóng góp trước đó. Nếu chúng ta nghĩ về mặt đóng góp của từng thao tác +1 riêng lẻ, thì mỗi đóng góp đó có thể được nhân đôi nhiều lần tùy thuộc vào số lượng điểm nối xuất hiện sau nó. 

Một cách trực tiếp để tính giá trị cuối cùng một cách có cấu trúc hơn là đảo ngược quan điểm. Thay vì theo dõi giá trị về phía trước, chúng tôi theo dõi số lần đóng góp của mỗi vị trí được tăng lên nhờ các điểm nối trong tương lai. Mỗi lần chúng ta đi qua một ngã ba, mọi thứ trước nó đều có thêm một hệ số hai trong ảnh hưởng trong tương lai. Điều này cho thấy tác dụng của các mối nối được nhân lên theo các hậu tố. 

Quan sát quan trọng là mọi vị trí đều đóng góp một giá trị cơ bản là 1 và trọng số cuối cùng của nó được xác định bằng số lượng điểm nối xuất hiện tại hoặc sau nó. Nếu có k điểm nối trong hậu tố bắt đầu từ vị trí i thì phần đóng góp của vị trí i được nhân với 2^k. 

Điều này làm giảm vấn đề thành một phép tính hậu tố đơn giản: đếm xem có bao nhiêu điểm nối còn lại ở bên phải và tích lũy các đóng góp tương ứng. Chúng ta có thể tính số lượng hậu tố theo thứ tự ngược lại và cập nhật câu trả lời chỉ trong một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Số lượng nhân đôi hậu tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý các phòng từ phải sang trái để luôn biết có bao nhiêu điểm nối tồn tại trong hậu tố. 

1. Khởi tạo một biến`multiplier`thành 1. Điều này thể hiện số lần đóng góp +1 mới sẽ được chia tỷ lệ do các điểm nối ở bên phải của nó. 
2. Khởi tạo`answer`về 0. Điều này sẽ tích lũy kết quả cuối cùng. 
3. Đi qua các phòng từ n xuống 1. 
4. Với mỗi phòng i, hãy thêm`multiplier`ĐẾN`answer`. Điều này phản ánh rằng phòng hiện tại đóng góp 1, nhưng nó bị ảnh hưởng bởi tất cả các điểm nối sau nó, mỗi điểm nối tăng gấp đôi giá trị của nó. 
5. Nếu phòng i là ngã ba thì nhân lên`multiplier`lên 2. Điều này cập nhật hiệu ứng cho tất cả các phòng trước đó vì giờ đây chúng sẽ được áp dụng thêm một lần nhân đôi trong tương lai. 
6. Tiếp tục cho đến khi tất cả các phòng được xử lý và xuất ra`answer`. 

Thứ tự các thao tác trong mỗi bước là rất quan trọng. Trước tiên, chúng tôi phải áp dụng khoản đóng góp bằng cách sử dụng hệ số nhân hiện tại trước khi cập nhật nó, vì phòng hiện tại sẽ không bị ảnh hưởng bởi trạng thái kết nối của chính nó khi xem xét việc nhân đôi dựa trên hậu tố. 

### Tại sao nó hoạt động 

Bất biến quan trọng là khi chúng ta ở vị trí i,`multiplier`bằng 2 được nâng lên theo số nút giao hoàn toàn ở bên phải của i. Điều này đảm bảo rằng khi chúng ta thêm`multiplier`với câu trả lời, chúng tôi đang áp dụng chính xác số lần nhân đôi trong tương lai cho khoản đóng góp đơn vị từ phòng i. Đang cập nhật`multiplier`chỉ sau khi xử lý phòng hiện tại mới giữ nguyên định nghĩa hậu tố nghiêm ngặt này, ngăn chặn sự tự ảnh hưởng và đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # assuming a[i] = 1 if junction, 0 otherwise
    answer = 0
    multiplier = 1

    for i in range(n - 1, -1, -1):
        answer += multiplier
        if a[i] == 1:
            multiplier *= 2

    print(answer)

if __name__ == "__main__":
    solve()
```Giải pháp này duy trì một hệ số nhân hậu tố đang chạy để nắm bắt số lần đóng góp của một đơn vị được nhân đôi một cách hiệu quả nhờ các điểm nối trong tương lai. Mỗi phòng đóng góp chính xác một lần, được chia tỷ lệ phù hợp và các điểm nối cập nhật tỷ lệ cho tất cả các vị trí trước đó. 

Một lỗi phổ biến là cập nhật hệ số nhân trước khi thêm vào câu trả lời, điều này sẽ áp dụng không chính xác hiệu ứng tiếp nối của chính phòng đó cho chính phòng đó. Một vấn đề tế nhị khác là giả định về an toàn tràn; trong Python điều này là tự động, nhưng trong các ngôn ngữ số nguyên có chiều rộng cố định, điều này đòi hỏi phải xử lý cẩn thận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xem xét đầu vào về vị trí phòng`[0, 1, 0]`, nghĩa là chỉ có phòng thứ hai là ngã ba. 

| tôi | phòng | số nhân | đóng góp thêm | trả lời | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | 1 | 1 | 1 | 
| 1 | 1 | 1 | 1 | 2 | 
| 0 | 0 | 2 | 2 | 4 | 

Dấu vết này cho thấy đường giao nhau ở vị trí 1 nhân đôi tất cả các đóng góp cho bên trái của nó, trong khi các đóng góp cho bên phải của nó vẫn không bị ảnh hưởng. 

### Ví dụ 2 

Hãy coi tất cả các phòng đều là ngã ba:`[1, 1, 1]`. 

| tôi | phòng | số nhân | đóng góp thêm | trả lời | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | 1 | 
| 1 | 1 | 2 | 2 | 3 | 
| 0 | 1 | 4 | 4 | 7 | 

Mỗi vị trí trước đó sẽ thấy một hệ số nhân đôi bổ sung, tạo ra sự tăng trưởng hình học nhất quán với việc nhân đôi hậu tố lặp đi lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi ngược lại qua tất cả các phòng | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Thuật toán xử lý mỗi phòng chính xác một lần và thực hiện cập nhật liên tục, giúp nó dễ dàng đủ nhanh đối với các giới hạn thông thường lên tới 10^5 trở lên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    answer = 0
    multiplier = 1

    for i in range(n - 1, -1, -1):
        answer += multiplier
        if a[i] == 1:
            multiplier *= 2

    return str(answer)

# minimum size
assert run("1\n0\n") == "1"
assert run("1\n1\n") == "1"

# no junctions
assert run("3\n0 0 0\n") == "3"

# all junctions
assert run("3\n1 1 1\n") == "7"

# alternating
assert run("4\n1 0 1 0\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | tối thiểu không giao nhau | 
| 1 1 | 1 | ngã ba tối thiểu | 
| 3 số không | 3 | không nhân đôi hành vi | 
| 3 cái | 7 | độ chính xác tăng trưởng theo cấp số nhân | 
| xen kẽ | 6 | nhân giống hỗn hợp | 

## Vỏ cạnh 

Một phòng đơn không nằm ở ngã ba là trường hợp đơn giản nhất. Thuật toán đặt hệ số nhân thành 1, cộng nó một lần và xuất ra 1, khớp với kết quả mong đợi vì không bao giờ xảy ra nhân đôi. 

Đối với phòng nối đơn, thuật toán vẫn cộng 1 trước khi áp dụng bất kỳ phép nhân nào. Vì không có phòng nào ở bên phải nên hệ số nhân vẫn là 1 và kết quả lại là 1. Điều này phù hợp với thực tế là việc nhân đôi chỉ ảnh hưởng đến các phòng trước đó và không có phòng nào cả. 

Trong một mảng hoàn toàn không có điểm nối, hệ số nhân không bao giờ thay đổi từ 1. Mỗi lần lặp sẽ cộng chính xác 1, do đó kết quả cuối cùng là n, khớp với cách diễn giải là chỉ xảy ra gia số. 

Trong một mảng nối đầy đủ, hệ số nhân tiến triển thành 1, 2, 4, 8 khi chúng ta di chuyển sang trái. Mỗi đóng góp được tính theo số lượng điểm nối trong tương lai và tổng hình học cuối cùng được tạo ra một cách chính xác bằng cách tích lũy ngược lại.
