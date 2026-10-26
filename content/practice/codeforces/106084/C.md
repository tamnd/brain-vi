---
title: "CF 106084C - Vực thẳm một chiều"
description: "Mỗi trường hợp thử nghiệm mô tả một hệ thống hang động thẳng đứng với một số trục. Bạn bắt đầu ở đầu trục đã chọn và di chuyển xuống dưới. Khi đi xuống, bạn có thể gặp những đường hầm nằm ngang được đặt ở độ sâu khác nhau."
date: "2026-06-22T18:57:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106084
codeforces_index: "C"
codeforces_contest_name: "2025 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 106084
solve_time_s: 74
verified: true
draft: false
---

[CF 106084C - Vực thẳm một chiều](https://codeforces.com/problemset/problem/106084/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một hệ thống hang động thẳng đứng với một số trục. Bạn bắt đầu ở đầu trục đã chọn và di chuyển xuống dưới. Khi đi xuống, bạn có thể gặp những đường hầm nằm ngang được đặt ở độ sâu khác nhau. Mỗi đường hầm nối hai trục và nằm ở một độ sâu cụ thể, với các đường hầm được sắp xếp từ trên xuống dưới. 

Quy tắc chuyển động là hạn chế chính. Khi bạn đến độ sâu của đường hầm, nếu bạn hiện đang ở một trong hai trục điểm cuối của nó, bạn buộc phải đi qua nó ngay lập tức tới trục kia. Nếu bạn không ở trong trục điểm cuối, đường hầm sẽ không ảnh hưởng đến đường đi của bạn. Bởi vì chuyển động luôn đi xuống và các đường hầm được xử lý theo thứ tự độ sâu tăng dần nên quỹ đạo của bạn hoàn toàn được xác định khi bạn chọn trục xuất phát. 

Mỗi đường hầm có một giá trị và bạn thu thập nó khi và chỉ khi bạn đi qua đường hầm đó trong quá trình đi xuống. Nhiệm vụ là chọn trục khởi động sao cho tổng giá trị thu được lớn nhất. 

Kích thước đầu vào lớn, với tổng số lên tới hai trăm nghìn trục và đường hầm trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ việc mô phỏng quy trình riêng biệt cho từng trục khởi động, vì điều đó sẽ dẫn đến trạng thái gần như O(nm) trong trường hợp xấu nhất, vượt xa giới hạn thời gian. 

Một số trường hợp góc có giá trị riêng biệt. 

Nếu không có đường hầm, câu trả lời luôn là 0 bất kể trục khởi động là gì, vì không có gì được thu thập. 

Nếu tất cả các đường hầm đều rời rạc về độ sâu nhưng kết nối các trục tùy ý thì quy trình vẫn buộc phải chuyển đổi xác định, nhưng mô phỏng mỗi lần khởi động đơn giản có thể giả định không chính xác sự độc lập giữa các đường hầm, mặc dù các đường hầm trước đó sẽ thay đổi liệu các đường hầm sau có được kích hoạt hay không. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ là khi một đường hầm thay đổi trục hiện tại, sau đó xác định liệu các đường hầm trong tương lai có được kích hoạt hay không. Ví dụ: bắt đầu từ trục 1 có thể cho phép bạn đi vào một đường hầm có giá trị sau đó, trong khi bắt đầu từ trục 2 có thể tránh hoàn toàn đường hầm đó, ngay cả khi cả hai lựa chọn ban đầu đều trông đối xứng. 

Khó khăn cốt lõi là mỗi đường hầm không chỉ thêm một giá trị cục bộ mà còn thay đổi trạng thái xác định các lần kích hoạt trong tương lai. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp cho mỗi trục khởi động sẽ theo dõi trục hiện tại khi chúng tôi quét các đường hầm từ trên xuống dưới. Đối với mỗi lần bắt đầu, chi phí này là O(m), mang lại tổng công là O(nm). Với n và m lên tới 2×10^5 thì phương pháp này không khả thi. 

Trở ngại chính là các trục khởi động khác nhau chỉ khác nhau ở cách cùng một chuỗi các lần hoán đổi cưỡng bức ảnh hưởng đến chúng. Trình tự đường hầm áp dụng cùng một quy tắc chuyển đổi cho mọi vị trí bắt đầu: tại mỗi đường hầm, chỉ có hai trạng thái hiện tại cụ thể là quan trọng và các trạng thái đó sẽ được hoán đổi nếu đạt đến. 

Thay vì mô phỏng từng lần xuất phát một cách độc lập, chúng tôi đảo ngược quan điểm. Tại bất kỳ thời điểm cố định nào trước khi xử lý đường hầm, mọi trục khởi động sẽ ánh xạ tới trục hiện tại. Ánh xạ này là một hoán vị trên các trục, bởi vì mỗi lần khởi động đều nằm trong chính xác một trục hiện tại. Mỗi đường hầm cập nhật hoán vị này bằng cách hoán đổi hình ảnh của hai điểm cuối. 

Quan sát này biến vấn đề thành việc duy trì một hoán vị động trong khi tích lũy các giá trị. Một đường hầm đóng góp giá trị của nó cho chính xác những trục xuất phát có hình ảnh hiện tại là một trong những điểm cuối của nó. Những trục khởi đầu đó chính xác là hình ảnh tiền đề của hai điểm cuối theo hoán vị hiện tại. 

Vì vậy, đối với mỗi đường hầm, chúng ta chỉ cần xác định hai vị trí bắt đầu hiện đang ánh xạ vào các điểm cuối của nó, thêm giá trị cho chúng rồi cập nhật hoán vị bằng cách hoán đổi các điểm cuối đó. 

Điều này làm giảm vấn đề đối với các bản cập nhật O(m), mỗi bản cập nhật trong O(1), bằng cách duy trì ánh xạ nghịch đảo của hoán vị.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force mỗi lần bắt đầu | O(nm) | O(1) | Quá chậm | 
| Duy trì hoán vị + cập nhật nghịch đảo | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các đường hầm theo thứ tự từ trên xuống dưới nhất định, duy trì hoán vị mô tả vị trí hiện tại của mỗi trục xuất phát sau khi xử lý tất cả các đường hầm trước đó. 

1. Khởi tạo một mảng`inv`, Ở đâu`inv[i] = i`. Điều này có nghĩa là ban đầu điểm bắt đầu ở trục i vẫn ở trục i trước khi bất kỳ đường hầm nào được xử lý. 
2. Đồng thời khởi tạo một mảng`score[i] = 0`, sẽ lưu tổng giá trị thu thập được cho trục khởi động i. 
3. Đối với mỗi hầm`(x, y, v)`theo thứ tự, trước tiên chúng ta xác định trục khởi động nào hiện đang nằm ở trục x và y. Đây chính xác là`inv[x]`Và`inv[y]`. 
4. Chúng tôi thêm`v`cho cả hai`score[inv[x]]`Và`score[inv[y]]`, bởi vì bất kỳ điểm bắt đầu nào hiện ở x hoặc y sẽ đi qua đường hầm này và thu thập giá trị của nó. 
5. Sau khi áp dụng đường hầm, chúng tôi cập nhật hoán vị để phản ánh chuyển động cưỡng bức. Mọi điểm bắt đầu hiện tại tại x đều di chuyển đến y và mọi điểm bắt đầu hiện tại tại y đều di chuyển đến x. Điều này được thực hiện bằng cách hoán đổi`inv[x]`Và`inv[y]`. 
6. Sau khi xử lý tất cả các đường hầm, câu trả lời là giá trị lớn nhất trong`score`. 

Điểm cấu trúc quan trọng là`inv`luôn biểu thị vị trí hiện tại của mỗi trục xuất phát dưới sự phát triển do các đường hầm trước đó gây ra. Mỗi đường hầm chỉ ảnh hưởng đến hai vị trí trong cấu trúc này, vì vậy các bản cập nhật luôn được cập nhật liên tục. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hàm ánh xạ trục khởi động tới vị trí hiện tại của nó là phép chiếu song song trên các trục. Mỗi đường hầm hoạt động như một sự chuyển vị trên không gian vị trí hiện tại, hoán đổi hai điểm cuối nếu chúng đang hoạt động. Trục bắt đầu đóng góp vào đường hầm khi và chỉ khi vị trí được ánh xạ hiện tại của nó bằng một trong các điểm cuối, tương ứng chính xác với việc nằm trong hình ảnh nghịch đảo của các điểm cuối đó theo hoán vị. Vì chúng tôi duy trì hoán vị này một cách chính xác nên mọi đóng góp chỉ được tính một lần và chỉ khi cần thiết, đồng thời bản cập nhật trao đổi sẽ duy trì tính chính xác cho tất cả các đường hầm trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        inv = list(range(n + 1))
        score = [0] * (n + 1)

        for _ in range(m):
            x, y, v = map(int, input().split())

            sx, sy = inv[x], inv[y]

            score[sx] += v
            score[sy] += v

            inv[x], inv[y] = inv[y], inv[x]

        out.append(str(max(score[1:])))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Chi tiết triển khai chính là hướng của hoán vị. Mảng`inv`được sử dụng như một ánh xạ nghịch đảo từ trục hiện tại đến trục khởi động. Đây là điều cho phép chúng tôi xác định trực tiếp điểm khởi động nào bị ảnh hưởng bởi mỗi đường hầm trong thời gian O(1) cho mỗi điểm cuối. 

Bước hoán đổi phải diễn ra sau khi cập nhật điểm số. Việc hoán đổi trước đó sẽ gán sai đường hầm cho các trục khởi đầu sai vì sự đóng góp phụ thuộc vào trạng thái trước đường hầm. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ với ba trục và hai đường hầm. Bắt đầu với bản đồ nhận dạng. 

| Bước | Đường hầm | inv trước | bắt đầu bị ảnh hưởng | cập nhật điểm số | gọi sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,2,3) | [1,2,3] | 1,2 | +3 đến 1 và 2 | [2,1,3] | 
| 2 | (2,3,9) | [2,1,3] | 1,3 | +9 đến 1 và 3 | [2,3,1] | 

Sau khi xử lý, điểm số sẽ trở thành: 

bắt đầu 1: 3 + 9 = 12 

bắt đầu 2: 3 

bắt đầu 3: 9 

Tối đa là 12, đạt được bằng cách bắt đầu từ trục 1. 

Dấu vết này cho thấy danh tính của “điểm bắt đầu hiện đang ở một trục” phát triển như thế nào theo thời gian và cách mỗi đường hầm chỉ chạm vào hai mục của ánh xạ đó. 

Bây giờ hãy xem xét trường hợp các đường hầm trao đổi qua lại, nhấn mạnh hành vi hoán vị. 

| Bước | Đường hầm | inv trước | bắt đầu bị ảnh hưởng | cập nhật điểm số | gọi sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,2,5) | [1,2,3] | 1,2 | +5 đến 1 và 2 | [2,1,3] | 
| 2 | (1,2,7) | [2,1,3] | 2,1 | +7 đến 2 và 1 | [1,2,3] | 

Ở đây, đường hầm thứ hai khôi phục một cách hiệu quả ánh xạ ban đầu, cho thấy hệ thống hoạt động giống như các chuyển vị lặp đi lặp lại trên một hoán vị chứ không phải là các hiệu ứng độc lập trên mỗi đường hầm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Mỗi đường hầm thực hiện công việc O(1): hai lần đọc mảng, hai phép cộng, một lần hoán đổi | 
| Không gian | O(n) | Mảng`inv`Và`score`lưu trữ trạng thái trên mỗi trục | 

Tổng các ràng buộc tổng n và m trong các trường hợp thử nghiệm là 2×10^5, do đó cách tiếp cận thời gian tuyến tính là đủ và thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, m = map(int, input().split())
        inv = list(range(n + 1))
        score = [0] * (n + 1)

        for _ in range(m):
            x, y, v = map(int, input().split())
            sx, sy = inv[x], inv[y]
            score[sx] += v
            score[sy] += v
            inv[x], inv[y] = inv[y], inv[x]

        res.append(str(max(score[1:])))
    return "\n".join(res) + ("\n" if res else "")

# sample-style sanity checks
assert run("1\n3 2\n1 2 3\n2 3 9\n") == "12\n"

# no tunnels
assert run("1\n5 0\n") == "0\n"

# single tunnel
assert run("1\n4 1\n2 4 10\n") == "10\n"

# chain swaps
assert run("1\n3 3\n1 2 5\n2 3 7\n1 3 2\n") == "14\n"

# symmetric case
assert run("1\n2 2\n1 2 4\n1 2 6\n") == "10\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có đường hầm | 0 | trường hợp cơ sở đúng đắn | 
| đường hầm đơn | giá trị | logic kích hoạt đơn | 
| hoán đổi chuỗi | 14 | tiến hóa hoán vị qua nhiều lần hoán đổi | 
| đối xứng lặp đi lặp lại | 10 | tích lũy và cập nhật lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp tối thiểu không có đường hầm xác nhận rằng thuật toán để lại tất cả các điểm ở mức 0 một cách chính xác và không truy cập vào các ánh xạ không hợp lệ. 

Trường hợp có một đường hầm duy nhất xác minh rằng cả hai điểm cuối đều đóng góp chính xác một lần và chỉ một lần vào trục khởi đầu tương ứng của chúng. 

Trình tự trong đó các đường hầm liên tục hoán đổi cùng một cặp trục chứng tỏ rằng trạng thái hoán vị thực sự là động và phải được cập nhật sau mỗi đường hầm, không được coi là kề cận tĩnh. 

Trường hợp tinh vi cuối cùng là khi một đường hầm có giá trị cao được bắt đầu bằng các hoán đổi di chuyển các trục khởi đầu khác nhau vào điểm cuối của nó. Thuật toán xử lý việc này một cách chính xác vì nó luôn truy vấn`inv[x]`Và`inv[y]`tại thời điểm xử lý, thay vì dựa vào nhận dạng trục ban đầu.
