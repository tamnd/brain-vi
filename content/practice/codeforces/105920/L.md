---
title: "CF 105920L - Đom đóm"
description: "Chúng ta được cung cấp một số vị trí ẩn trên trục số từ 1 đến 100000. Những vị trí này thể hiện vị trí của các nhóm đom đóm. Chúng tôi không biết vị trí và nhiều nhóm có thể tồn tại ở các điểm khác nhau."
date: "2026-06-21T15:34:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105920
codeforces_index: "L"
codeforces_contest_name: "Soy Cup #1: Firefly"
rating: 0
weight: 105920
solve_time_s: 50
verified: true
draft: false
---

[CF 105920L - Fyrefly](https://codeforces.com/problemset/problem/105920/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số vị trí ẩn trên trục số từ 1 đến 100000. Những vị trí này thể hiện vị trí của các nhóm đom đóm. Chúng tôi không biết vị trí và nhiều nhóm có thể tồn tại ở các điểm khác nhau. 

Thao tác duy nhất được phép là chọn tọa độ x và nhận được phản hồi bằng tổng khoảng cách từ x đến tất cả các vị trí ẩn. Mỗi câu trả lời là tổng các khác biệt tuyệt đối giữa x và mọi điểm ẩn. Mục tiêu là xác định ít nhất một trong các vị trí ẩn bằng cách sử dụng tối đa 40 truy vấn như vậy cho mỗi trường hợp thử nghiệm. 

Chi tiết cấu trúc quan trọng là hàm phản hồi hoạt động giống như hàm lồi trên các số nguyên. Khi chúng ta di chuyển x dọc theo đường thẳng, tổng khoảng cách sẽ giảm cho đến khi chúng ta chạm đến “tâm” của sự phân bổ điểm và sau đó tăng lên. Trung tâm này tương ứng với trung vị của nhiều vị trí ẩn. Bất kỳ vị trí trung tuyến nào cũng được đảm bảo là một trong những tọa độ thực tế trong bài toán này vì tất cả các điểm đều là số nguyên trên một đường rời rạc. 

Các ràng buộc ngụ ý rằng n có thể lớn tới 10000 cho mỗi trường hợp thử nghiệm và có thể có tới 1000 trường hợp thử nghiệm, nhưng tổng của n bị giới hạn. Phạm vi tọa độ được cố định ở mức 100000. Điều này có nghĩa là chúng tôi không thể chấp nhận bất kỳ phương pháp nào cố gắng tái tạo lại tất cả các điểm một cách rõ ràng hoặc truy vấn mọi vị trí. Thay vào đó, mỗi bài kiểm tra phải được giải bằng truy vấn logarit trong không gian tọa độ, sử dụng cấu trúc của hàm khoảng cách tuyệt đối. 

Một sự hiểu lầm ngây thơ là cho rằng chúng ta có thể trực tiếp “đảo ngược” một truy vấn. Một truy vấn chỉ cung cấp tổng hợp toàn cầu và không tiết lộ cấu trúc cục bộ. Một trường hợp thất bại tinh vi khác xuất hiện nếu người ta cho rằng điểm phản hồi tối thiểu là duy nhất trong mọi trường hợp. Khi tất cả các điểm giống hệt nhau, mọi truy vấn đều trả về cùng tọa độ với câu trả lời và tìm kiếm nhị phân vẫn phải hội tụ chính xác mà không dựa vào nhiễu độ dốc. 

## Phương pháp tiếp cận 

Tư duy vũ phu sẽ cố gắng thăm dò mọi tọa độ x từ 1 đến 100000 và tính toán phản hồi. Điều này sẽ xác định rõ ràng mức tối thiểu của hàm và mức tối thiểu đó tương ứng với vị trí trung vị. Tuy nhiên, điều này yêu cầu 100000 truy vấn cho mỗi trường hợp thử nghiệm, vượt xa giới hạn 40 truy vấn, do đó, nó thất bại ngay lập tức ngay cả trước khi xem xét nhiều trường hợp thử nghiệm. 

Quan sát quan trọng là hàm f(x) được định nghĩa là tổng các khoảng cách tuyệt đối có đạo hàm riêng biệt có thể dự đoán được. Nếu chúng ta xét f thay đổi như thế nào khi di chuyển từ x đến x+1, mỗi điểm ẩn đóng góp +1 nếu nó nằm ở hoặc bên trái x và đóng góp -1 nếu nó nằm bên phải x. Điều này biến đổi hiệu f(x+1) − f(x) thành một đại lượng mã hóa trực tiếp số lượng điểm nằm trên mỗi cạnh của x. 

Điều này có nghĩa là chúng ta có thể khôi phục số tiền tố của các điểm lên tới x chỉ bằng hai đánh giá của hàm. Khi chúng ta có thể đánh giá có bao nhiêu điểm ẩn ≤ x, chúng ta có thể tìm kiếm nhị phân cho x nhỏ nhất sao cho số này đạt ít nhất (n+1)/2, là vị trí trung vị. Tọa độ đó phải trùng với một trong những vị trí đom đóm ẩn nấp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force quét tất cả x | Truy vấn O(100000) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân sử dụng phục hồi độ dốc | Truy vấn O(log 100000) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào thực tế là chúng tôi có thể tính toán số tiền tố bằng cách sử dụng sai phân của hàm khoảng cách.

1. Chúng tôi xác định hàm f(x) truy vấn bộ tương tác và trả về tổng khoảng cách đến tất cả các điểm ẩn. Chúng tôi coi đây là đánh giá hộp đen. 
2. Với mọi x, chúng ta tính f(x) và f(x+1). Từ hai giá trị này, chúng ta tính số điểm ẩn nhỏ hơn hoặc bằng x bằng cách sử dụng đồng nhất thức count_le(x) = (f(x+1) − f(x) + n) / 2. Điều này hiệu quả vì mỗi điểm đóng góp +1 hoặc -1 vào hiệu số tùy thuộc vào phía nào của x. 
3. Chúng ta tìm kiếm nhị phân trên phạm vi tọa độ [1, 100000] để tìm x nhỏ nhất sao cho count_le(x) ít nhất là (n+1)//2. Điều này đảm bảo chúng tôi đạt đến ngưỡng trung bình. 
4. Trong quá trình tìm kiếm nhị phân, mỗi điểm giữa yêu cầu hai truy vấn, một ở giữa và một ở giữa+1, vì vậy chúng tôi cẩn thận đảm bảo rằng chúng tôi không vượt quá giới hạn 40 truy vấn. 
5. Khi tìm kiếm nhị phân hội tụ, chúng tôi xuất tọa độ đó làm câu trả lời. 

Lý do điều này hoạt động là vì hàm f(x) lồi và đạo hàm rời rạc của nó thay đổi đơn điệu từ âm sang dương khi x vượt qua danh sách các điểm ẩn đã được sắp xếp. Số tiền tố được trích ra từ sự khác biệt là đơn điệu, làm cho tìm kiếm nhị phân hợp lệ. Ngưỡng trung bình đảm bảo rằng chúng ta hạ cánh tại một điểm thực sự có ít nhất một nhóm đom đóm chiếm giữ, chứ không chỉ là tâm hình học giữa chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())

        def query(x):
            print("?", x)
            sys.stdout.flush()
            return int(input())

        def pref_le(x):
            fx = query(x)
            fx1 = query(x + 1)
            return (fx1 - fx + n) // 2

        lo, hi = 1, 100000
        target = (n + 1) // 2

        while lo < hi:
            mid = (lo + hi) // 2
            if pref_le(mid) >= target:
                hi = mid
            else:
                lo = mid + 1

        print("!", lo)
        sys.stdout.flush()

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng xung quanh việc chuyển đổi truy vấn khoảng cách toàn cầu thành tín hiệu cấu trúc cục bộ. Hàm pref_le tái tạo lại số lượng điểm ẩn nằm ở bên trái của một vị trí chỉ bằng hai lệnh gọi tương tác. Đây là sự chuyển đổi quan trọng biến vấn đề thành một tìm kiếm nhị phân tiêu chuẩn trên một vị từ đơn điệu. 

Tìm kiếm nhị phân trên miền tọa độ chứ không phải trên các chỉ mục vì chúng ta không có quyền truy cập vào danh sách các vị trí được sắp xếp. Mỗi bước quyết định đều hợp lệ vì số tiền tố là đơn điệu trong x, vì vậy một khi chúng ta vượt qua ngưỡng trung bình, chúng ta sẽ không bao giờ quay trở lại dưới ngưỡng đó. 

Chức năng truy vấn được tách biệt để đảm bảo quá trình xả xảy ra ngay sau mỗi lần tương tác, điều này cần thiết để tránh lỗi không hoạt động trong các vấn đề tương tác. 

## Ví dụ đã hoạt động 

Vì đây là tính tương tác nên chúng tôi mô phỏng ý tưởng bằng cách sử dụng một mảng ẩn cố định. 

Giả sử vị trí ẩn là [1, 5, 10, 8, 9, 6, 13] nên n = 7 và trung vị là vị trí 8. 

Chúng tôi chỉ ra cách tìm kiếm nhị phân sẽ tiến hành về mặt khái niệm. 

| bước | lo | xin chào | giữa | pref_le(giữa) | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 100000 | 50000 | nhỏ (7) | đi bên trái | 
| 2 | 1 | 50000 | 25000 | nhỏ | đi bên trái | 
| 3 | 1 | 25000 | 12500 | nhỏ | đi bên trái | 
| ... | ... | ... | ... | ... | ... | 
| k | 1 | 10 | 5 | 3 | đi bên phải | 
| k+1 | 6 | 10 | 8 | 4 | dừng lại | 

Tại x = 8, số tiền tố đạt ít nhất là 4, tức là (n+1)//2 = 4, do đó việc tìm kiếm ổn định ở mức 8. 

Dấu vết này chứng tỏ rằng chúng ta không bao giờ cần xác định vị trí tất cả các điểm một cách rõ ràng. Chúng tôi chỉ cần đủ độ phân giải để xác định việc vượt qua ngưỡng trung bình. 

Kịch bản thứ hai là khi tất cả các điểm đều bằng nhau, chẳng hạn [42, 42, 42]. Mọi truy vấn đều trả về một hàm tuyến tính đối xứng có tâm ở 42. Số tiền tố nhảy từ 0 đến 7 tại x = 42, do đó tìm kiếm nhị phân hội tụ chính xác đến 42 mà không có sự mơ hồ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log 100000) mỗi lần kiểm tra | Mỗi bước tìm kiếm nhị phân sử dụng các truy vấn tương tác liên tục | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến | 

Phạm vi tọa độ được cố định và đủ nhỏ để tìm kiếm logarit dễ dàng phù hợp trong phạm vi 40 truy vấn cho mỗi trường hợp thử nghiệm. Vì mỗi bước sử dụng hai truy vấn nên khoảng 15 đến 17 bước là đủ, nằm trong giới hạn. 

## Trường hợp thử nghiệm 

Vì đây là một vấn đề tương tác nên chúng tôi mô phỏng một oracle cục bộ để trả lời các truy vấn khoảng cách.```python
import sys, io

def make_solver(hidden):
    def run(inp: str) -> str:
        data = inp.strip().split()
        t = int(data[0])
        idx = 1
        out_lines = []

        for _ in range(t):
            n = int(data[idx]); idx += 1

            def f(x):
                return sum(abs(v - x) for v in hidden)

            def pref_le(x):
                fx = f(x)
                fx1 = f(x + 1)
                return (fx1 - fx + n) // 2

            lo, hi = 1, 100000
            target = (n + 1) // 2

            while lo < hi:
                mid = (lo + hi) // 2
                if pref_le(mid) >= target:
                    hi = mid
                else:
                    lo = mid + 1

            out_lines.append(str(lo))

        return "\n".join(out_lines)

    return run

# custom tests
run = make_solver([5])
assert run("1\n1\n") == "5"

run = make_solver([1, 5, 10])
assert run("1\n3\n") == "5"

run = make_solver([42, 42, 42])
assert run("1\n3\n") == "42"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | cùng giá trị | độ đúng cơ sở | 
| nhiều điểm lan truyền | lựa chọn trung bình | tính đúng đắn của logic trung vị | 
| mọi điểm bằng nhau | giá trị đó | ổn định theo hàm phẳng | 

## Vỏ cạnh 

Khi tất cả các điểm ẩn giống hệt nhau, hàm f(x) trở thành hàm hình chữ V đơn giản có tâm tại tọa độ đó. Việc tái tạo tiền tố vẫn hoạt động vì hiệu f(x+1) − f(x) nhảy từ âm sang dương chính xác tại điểm đó. Do đó tìm kiếm nhị phân hội tụ mà không có dao động. 

Khi n chẵn, có hai đường trung tuyến theo nghĩa cổ điển, nhưng cả hai đều tương ứng với tọa độ hợp lệ giữa các điểm ẩn trong bài toán này. Thuật toán nhắm vào trung vị thấp hơn thông qua (n+1)//2, đảm bảo sự hội tụ tất định cho câu trả lời hợp lệ. 

Khi tất cả các điểm nằm gần ranh giới 1 hoặc 100000, tìm kiếm nhị phân vẫn hoạt động chính xác vì các truy vấn vẫn nằm trong giới hạn và tính đơn điệu của hàm tiền tố được giữ nguyên ngay cả ở các cạnh.
