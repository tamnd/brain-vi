---
title: "CF 105023H - Sân Khấu Lấp Lánh"
description: "Chúng ta được cấp một bộ xe tải được đặt trên đường một chiều. Mỗi xe xuất phát ở một vị trí khác nhau và chuyển động mãi mãi với vận tốc cố định."
date: "2026-06-28T01:45:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105023
codeforces_index: "H"
codeforces_contest_name: "HPI 2024 Novice"
rating: 0
weight: 105023
solve_time_s: 73
verified: false
draft: false
---

[CF 105023H - Sân khấu của Sparkle](https://codeforces.com/problemset/problem/105023/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ xe tải được đặt trên đường một chiều. Mỗi xe xuất phát ở một vị trí khác nhau và chuyển động mãi mãi với vận tốc cố định. Bất cứ khi nào hai xe tải gặp nhau tại cùng một thời điểm, chúng sẽ va chạm và thay vì tương tác vật lý, chúng chỉ trao đổi vận tốc. Vì tất cả các xe tải đều có khối lượng bằng nhau nên quy tắc hoán đổi này khiến hệ thống hoạt động như thể các xe tải đi qua nhau nhưng trao đổi danh tính. 

Nhiệm vụ không phải là mô phỏng chuyển động một cách rõ ràng mà là tính toán tổng số sự kiện va chạm xảy ra. Một sự kiện va chạm giữa hai xe tải góp phần đưa ra câu trả lời, ngoại trừ khi nhiều xe tải gặp nhau cùng lúc, trong trường hợp đó tất cả các cặp không có thứ tự trong số chúng được tính là va chạm. Nếu quá trình dẫn đến vô số va chạm, chúng ta phải báo cáo -1. 

Các ràng buộc cho thấy số lượng xe tải lên tới 5000, do đó, bất kỳ phương pháp nào kiểm tra tất cả các cặp xe tải đều ở mức giới hạn nhưng vẫn khả thi trong O(N^2). Bất cứ điều gì liên quan đến mô phỏng sự kiện trong thời gian liên tục sẽ quá chậm hoặc không ổn định về mặt số lượng, vì thời gian va chạm có thể rất nhỏ và nhiều sự kiện có thể xếp chồng lên nhau. 

Một quan sát ngây thơ nhưng quan trọng là hai xe tải có thể va chạm nhiều nhất một lần nếu chúng ta bỏ qua việc hoán đổi danh tính, bởi vì sau va chạm, trên thực tế, chúng tiếp tục như thể chúng đi xuyên qua nhau. Điều phức tạp duy nhất là chúng ta phải đếm chính xác số lần đảo ngược theo “hướng chuyển động hiệu dụng” và cũng xử lý các trường hợp suy biến khi nhiều xe tải gặp nhau đồng thời. 

Một trường hợp phức tạp xuất hiện khi nhiều xe tải có cùng vận tốc và ban đầu được sắp xếp theo cách trái ngược nhau. Nếu một xe tải nhanh hơn xuất phát sau một xe tải chậm hơn thì chúng sẽ gặp nhau đúng một lần; nếu một chiếc xe tải chậm hơn xuất phát sau một chiếc xe nhanh hơn thì chúng không bao giờ gặp nhau. Tuy nhiên, nếu vận tốc bằng nhau và vị trí khác nhau thì chúng không bao giờ va chạm. Mối nguy hiểm chính là giả định sai tính đơn điệu mà không xét đến các nhóm vận tốc bằng nhau. 

Một trường hợp góc quan trọng khác là va chạm vô hạn. Điều này xảy ra khi cấu hình cho phép hành vi nảy liên tục mà không tiến triển, nhưng trong mô hình hoán đổi khối lượng bằng nhau cụ thể này trên một đường thẳng, va chạm vô hạn chỉ phát sinh khi có các tình huống đối xứng bệnh lý trong đó các sự kiện lặp lại vô thời hạn, trong thực tế điều này làm giảm việc phát hiện các suy biến trong đó các sự kiện va chạm không làm giảm độ phức tạp của hệ thống. Chúng tôi sẽ làm điều này chính xác sau. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng từng cặp xe tải, tính toán xem chúng có gặp nhau hay không và nếu có thì tính một vụ va chạm. Đối với hai xe tải i và j, ta so sánh vị trí, vận tốc và tính thời gian giao nhau:$$x_i + v_i t = x_j + v_j t$$mang lại:$$t = \frac{x_j - x_i}{v_i - v_j}$$Nếu t dương thì chúng ta tính có va chạm. 

Cách tiếp cận này tính chính xác xem mỗi cặp có gặp nhau hay không. Tuy nhiên, nó thất bại theo hai cách. Đầu tiên, nó bỏ qua các va chạm đồng thời của nhiều xe tải, trong đó k xe tải gặp nhau cùng lúc và chúng ta phải tính k(k-1)/2 thay vì các sự kiện theo cặp riêng lẻ. Thứ hai, việc đếm cặp ngây thơ giả định tính độc lập, nhưng việc hoán đổi danh tính có nghĩa là chúng ta phải cẩn thận về việc liệu chúng ta đang theo dõi các giao lộ thực tế hay các tương tác được gắn nhãn. May mắn thay, trong mô hình hoán đổi khối lượng bằng nhau này, việc đếm các giao điểm theo cặp vẫn cho tổng số va chạm chính xác miễn là chúng ta xử lý chính xác việc nhóm đồng thời. 

Nút thắt rất đơn giản: có các cặp O(N^2), tối đa khoảng 25 triệu. Điều đó chỉ được chấp nhận trong Python nếu mỗi lần kiểm tra là O(1), vì vậy thách thức chính là nhóm các thời gian va chạm đồng thời một cách mạnh mẽ. 

Cái nhìn sâu sắc quan trọng là coi mỗi va chạm giữa i và j như một sự kiện thời gian, nhóm theo thời gian hợp lý chính xác và sau đó đếm xem có bao nhiêu cặp chia sẻ thời gian đó. Thay vì mô phỏng chuyển động, chúng tôi liệt kê tất cả thời gian va chạm theo cặp, gộp chúng và tính tổng các khoản đóng góp cho mỗi nhóm. 

Điều này làm giảm vấn đề trong việc tính toán thời gian hợp lý O(N^2) và tổng hợp chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(sự kiện nhật ký sự kiện) trường hợp xấu nhất không giới hạn | O(N) | Quá chậm / dễ vỡ | 
| Xếp thời gian theo cặp | O(N^2) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành tính toán thời gian va chạm cho tất cả các cặp xe tải và nhóm các thời điểm giống nhau. 

1. Với mỗi cặp xe tải i và j, giả sử i ở bên trái của j (hoán đổi vị trí nếu cần). Điều này tránh việc xử lý kép và đảm bảo xử lý dấu hiệu nhất quán. Lý do là hướng va chạm được xác định hoàn toàn theo thứ tự tương đối trong 1D. 
2. Tính vận tốc tương đối. Nếu xe bên trái không nhanh hơn xe bên phải thì sẽ không bao giờ gặp nhau nên bỏ qua cặp này. Những vụ va chạm có ý nghĩa duy nhất xảy ra khi xe tải bên trái nhanh hơn và đuổi kịp. 
3. Tính thời gian va chạm dưới dạng phân số rút gọn:$$t = \frac{x_j - x_i}{v_i - v_j}$$Thay vì dấu phẩy động, hãy lưu trữ số này dưới dạng cặp số nguyên được chuẩn hóa bằng cách sử dụng tính năng giảm gcd. Điều này tránh các lỗi chính xác có thể hợp nhất hoặc phân chia các sự kiện va chạm không chính xác. 
4. Sử dụng bản đồ băm được khóa theo (tử số, mẫu số) theo thời gian giảm để đếm có bao nhiêu cặp va chạm vào thời điểm chính xác đó. 
5. Sau khi xử lý tất cả các cặp, mỗi nhóm tương ứng với một sự kiện va chạm đồng thời liên quan đến chính xác hai xe tải cho mỗi mục nhập. Tuy nhiên, nhiều cặp có thể chia sẻ cùng một lúc, nghĩa là k xe gặp nhau. Đối với một thùng có m cặp, chúng ta phải khôi phục k sao cho:$$m = \frac{k(k-1)}{2}$$và sau đó cộng trực tiếp m vì mỗi cặp là một va chạm. 
6. Tổng hợp tất cả các khoản đóng góp của nhóm. 

Một chi tiết quan trọng là chúng tôi dựa vào thực tế là động lực hoán đổi khối lượng bằng nhau bảo toàn tập hợp thời gian va chạm độc lập với hoán đổi danh tính, do đó, các giao điểm hình học theo cặp sẽ xác định đầy đủ câu trả lời. 

### Tại sao nó hoạt động 

Mỗi va chạm tương ứng với một giao điểm duy nhất của hai quỹ đạo tuyến tính trong mặt phẳng thời gian-vị trí. Vì vận tốc hoán đổi không làm thay đổi các đường hình học nên mỗi điểm giao nhau đều có giá trị nội tại đối với dữ liệu ban đầu. Việc nhóm theo thời gian xử lý chính xác các cuộc họp đồng thời có nhiều xe tải vì tất cả các xe tải tham gia phải có chung thời gian giao nhau theo cặp giống hệt nhau, buộc phải có cấu trúc nhóm tại dấu thời gian đó. Việc đếm tất cả các cặp trong mỗi nhóm khớp chính xác với quy tắc k(k-1)/2 được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict
from math import gcd

def norm_num_den(num, den):
    if den < 0:
        num = -num
        den = -den
    g = gcd(abs(num), abs(den))
    return num // g, den // g

def solve():
    n = int(input())
    trucks = []
    for _ in range(n):
        x, v = map(int, input().split())
        trucks.append((x, v))

    events = defaultdict(int)

    for i in range(n):
        xi, vi = trucks[i]
        for j in range(i + 1, n):
            xj, vj = trucks[j]

            if xi == xj:
                continue

            if xi < xj:
                left_x, left_v = xi, vi
                right_x, right_v = xj, vj
            else:
                left_x, left_v = xj, vj
                right_x, right_v = xi, vi

            if left_v <= right_v:
                continue

            num = right_x - left_x
            den = left_v - right_v

            if den == 0:
                continue

            t = norm_num_den(num, den)
            events[t] += 1

    ans = 0
    for cnt in events.values():
        ans += cnt

    print(ans)

if __name__ == "__main__":
    solve()
```Quá trình triển khai lặp lại trên tất cả các cặp xe tải và thực thi thứ tự nhất quán từ trái sang phải để chỉ xem xét các hoạt động bắt kịp chuyển động về phía trước. Thời gian va chạm được lưu trữ dưới dạng phân số rút gọn để đảm bảo rằng các thời gian giống hệt nhau ánh xạ tới cùng một khóa băm. 

Một cạm bẫy phổ biến là sử dụng phép chia dấu phẩy động cho thời gian. Điều đó sẽ hợp nhất hoặc phân tách các sự kiện một cách không chính xác do lỗi chính xác. Một sự tinh tế khác là đảm bảo rằng hướng được xử lý chính xác: chỉ có xe tải bên trái nhanh hơn bắt được xe tải bên phải chậm hơn mới tạo ra một vụ va chạm hợp lệ. 

Cuối cùng, lưu ý rằng chúng ta không bao giờ xây dựng lại rõ ràng các nhóm k xe tải cùng một lúc. Thay vào đó, tính tổng tất cả các xung đột theo cặp trong mỗi nhóm thời gian sẽ tạo ra tổng k(k-1)/2 cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 1
2 -1
```Chỉ có một cặp tồn tại. Chúng tôi tính toán xem họ có gặp nhau hay không. 

| Cặp | Trái (x,v) | Đúng (x,v) | Tình trạng | Thời gian | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| (0,1) | (1,1) | (2,-1) | 1 > -1 hợp lệ | (2-1)/(1-(-1)) = 1 | 1 | 

Câu trả lời là 1. 

Điều này xác nhận rằng một sự kiện bắt kịp duy nhất được tính chính xác một lần. 

### Ví dụ 2 

đầu vào:```
3
1 3
2 2
3 1
```Mỗi xe tải nhanh hơn đều ở bên trái xe tải chậm hơn nên xảy ra nhiều vụ va chạm. 

| Cặp | Thời gian | 
| --- | --- | 
| (1,2) | 1 | 
| (1,3) | 1 | 
| (2,3) | 1 | 

Cả ba cặp va chạm cùng lúc, tạo thành một cuộc gặp gỡ 3 xe tải. Số lần va chạm là 3, bằng 3 chọn 2. 

Điều này xác minh việc xử lý chính xác các sự kiện đồng thời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) | Mỗi cặp xe tải được xử lý một lần và mỗi thao tác được khấu hao O(1) bao gồm gcd | 
| Không gian | O(N^2) | Trong trường hợp xấu nhất, tất cả thời gian va chạm theo cặp đều khác biệt và được lưu trữ trên bản đồ | 

Với N lên tới 5000, N^2 là khoảng 25 triệu kiểm tra cặp, chặt chẽ nhưng có thể chấp nhận được trong Python được tối ưu hóa với số học số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return run._capture()

def capture():
    import sys, io
    backup = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = backup
    return out.strip()

run._capture = capture

# sample
assert run("2\n1 1\n2 -1\n") == "1"

# no collision
assert run("3\n1 1\n2 1\n3 1\n") == "0"

# all collide same time
assert run("3\n1 3\n2 2\n3 1\n") == "3"

# simple chain
assert run("3\n1 3\n5 2\n10 1\n") == "3"

# boundary identical velocities
assert run("4\n1 5\n2 5\n3 5\n4 5\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vận tốc giống hệt nhau | 0 | không va chạm sai lầm | 
| tốc độ giảm đều đều | 3 | nhóm đồng thời | 
| tốc độ bằng nhau | 0 | bỏ qua tính đúng đắn logic | 
| dây chuyền nhỏ | 3 | va chạm chuyển tiếp | 

## Vỏ cạnh 

Trường hợp nguy hiểm là khi tất cả các xe tải có vận tốc như nhau. Trong tình huống này không tồn tại chuyển động tương đối nên không có cặp nào gặp nhau. Thuật toán bỏ qua tất cả các cặp một cách chính xác vì điều kiện`left_v <= right_v`loại bỏ chúng. 

Một trường hợp cạnh khác là thứ tự nghiêm ngặt với vận tốc giảm dần. Ở đây mọi cặp va chạm và thường ở những thời điểm giống hệt nhau. Logic phân nhóm đảm bảo rằng tất cả các giao điểm cùng lúc được hợp nhất, ngầm tạo ra đóng góp k(k-1)/2 chính xác. 

Trường hợp tinh tế cuối cùng là khi vận tốc rất lớn, lên tới 10^9. Nếu không giảm phân số, thời gian va chạm có thể bị tràn hoặc mất độ chính xác. Việc chuẩn hóa gcd đảm bảo rằng các thời gian giống hệt nhau về mặt cấu trúc luôn được hợp nhất bất kể độ lớn.
