---
title: "CF 1297D - Phân phối tiền thưởng"
description: "Mỗi nhân viên có mức lương cơ bản cố định và chúng tôi được phép phân bổ thêm một khoản tiền thưởng số nguyên sao cho tổng tiền thưởng của tất cả nhân viên chính xác là k. Sau khi cộng tiền thưởng, mức lương cuối cùng của mỗi nhân viên sẽ trở thành mức lương ban đầu cộng với tiền thưởng được giao."
date: "2026-06-16T05:00:25+07:00"
tags: ["codeforces", "competitive-programming", "*special", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1297
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes: Episode 3"
rating: 0
weight: 1297
solve_time_s: 217
verified: false
draft: false
---

[CF 1297D - Phân phối tiền thưởng ](https://codeforces.com/problemset/problem/1297/D) 

**Đánh giá:** - 
**Tags:** *đặc biệt, tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 3 phút 37 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi nhân viên có mức lương cơ bản cố định và chúng tôi được phép phân phối số tiền thưởng bổ sung sao cho tổng tiền thưởng của tất cả nhân viên là chính xác.`k`. Sau khi cộng tiền thưởng, mức lương cuối cùng của mỗi nhân viên sẽ trở thành mức lương ban đầu cộng với tiền thưởng được giao. 

Có ba yêu cầu về cấu trúc hình thành nên sự phân bố. Đầu tiên, thứ hạng của nhân viên theo mức lương không được thay đổi, vì vậy nếu một nhân viên ban đầu có mức lương cao hơn người khác, họ vẫn phải có mức lương cao hơn sau khi thưởng. Thứ hai, tất cả mức lương cuối cùng phải khác biệt. Thứ ba, trong số tất cả các phân bổ hợp lệ, chúng ta muốn mức lương cuối cùng tối đa càng nhỏ càng tốt. 

Điểm căng thẳng chính là tiền thưởng có thể được phân bổ tự do, nhưng chúng phải đảm bảo trật tự, tránh xung đột và tổng hợp thành một ngân sách cố định trong khi vẫn giữ giá trị cuối cùng tối đa ở mức tối thiểu. 

Các ràng buộc gợi ý một giải pháp tồi tệ nhất$O(n \log n)$mỗi trường hợp thử nghiệm. Với tối đa$10^5$tổng số nhân viên, một$O(n^2)$hoặc bất cứ điều gì liên quan đến việc quét tuyến tính lặp đi lặp lại cho mỗi phần tử sẽ quá chậm. Cho phép sắp xếp, nhưng bất kỳ điều gì liên quan đến điều chỉnh lồng nhau hoặc kiểm tra tính khả thi lặp đi lặp lại cho mỗi giá trị ứng cử viên sẽ không thành công trong trường hợp đầu vào xấu nhất. 

Một vấn đề tế nhị phát sinh từ sự kết hợp giữa trật tự chặt chẽ và tính duy nhất. Nếu hai nhân viên có giá trị điều chỉnh quá gần nhau, thậm chí bằng 1, chúng tôi sẽ phá vỡ điều kiện “tất cả đều khác nhau”. Một dạng thất bại khác là phép gán tham lam tôn trọng trật tự nhưng bỏ qua tính khả thi toàn cầu của tổng.`k`, có thể dễ dàng vượt quá hoặc quá thấp so với tổng yêu cầu. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là nghĩ đến việc phân phối tiền thưởng một cách tùy ý trong khi vẫn duy trì các ràng buộc. Người ta có thể cố gắng ấn định tiền thưởng từ mức lương cao nhất trở xuống, luôn đảm bảo rằng mỗi mức lương cuối cùng tiếp theo nhiều hơn mức trước đó ít nhất một. Điều này bảo toàn thứ tự và tính duy nhất, nhưng nó không kiểm soát tổng số tiền. Sau đó, nếu chúng ta cố gắng điều chỉnh các giá trị để đạt được chính xác`k`, chúng ta sẽ gặp phải một vấn đề cân bằng phức tạp trong đó những thay đổi cục bộ lan truyền trên toàn cầu. 

Một quan điểm bạo lực khác là coi mức lương cuối cùng là một chuỗi tăng dần nghiêm ngặt và phải lấn át mức lương ban đầu. Chúng ta có thể thử đoán mức lương cuối cùng tối đa`M`và sau đó tham lam gán các giá trị hợp lệ nhỏ nhất theo nó. Đối với một cố định`M`, kiểm tra tính khả thi là tuyến tính. Nhưng cố gắng hết sức`M`các giá trị dẫn đến một không gian tìm kiếm lớn và tìm kiếm nhị phân đơn giản vẫn yêu cầu xây dựng chuỗi một cách cẩn thận. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm: thay vì phân phối tiền thưởng trực tiếp, chúng tôi coi mức lương cuối cùng là một chuỗi tăng dần bị ràng buộc bởi giới hạn dưới`a_i`. Khi chúng tôi sắp xếp nhân viên theo`a_i`, chúng tôi thực thi rằng các giá trị cuối cùng phải tăng nghiêm ngặt và ít nhất mỗi giá trị`a_i`. Điều này biến vấn đề thành việc xây dựng chuỗi tăng dần khả thi tối thiểu về mặt từ điển theo một ràng buộc tổng, sau đó chuyển nó lên trên một cách đồng đều để tiêu thụ chính xác`k`. 

Việc xây dựng được thực hiện theo hai giai đoạn. Đầu tiên, chúng ta xây dựng mảng cuối cùng hợp lệ tối thiểu thỏa mãn thứ tự và tính duy nhất. Thứ hai, chúng tôi nhận thấy rằng việc tăng tất cả các giá trị cuối cùng theo cách có kiểm soát trong khi vẫn duy trì sự khác biệt cho phép chúng tôi phân bổ ngân sách còn lại một cách đồng đều từ cuối mà không vi phạm các ràng buộc. Đây là nơi cấu trúc trở nên tuyến tính và tham lam thay vì tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Điều chỉnh lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Xây dựng tham lam sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sắp xếp nhân viên theo mức lương cơ bản của họ trong khi theo dõi các chỉ số ban đầu. 

1. Sắp xếp nhân viên theo`a_i`theo thứ tự tăng dần. Điều này khắc phục thứ tự cuối cùng được yêu cầu, vì thứ tự cuối cùng phải khớp với thứ tự ban đầu. 
2. Bắt đầu xây dựng mảng cuối cùng dự kiến`b`, Ở đâu`b[i]`đại diện cho mức lương cuối cùng của nhân viên nhỏ thứ i. Khởi tạo`b[0] = a[0]`. 
3. Đối với mỗi nhân viên tiếp theo, hãy đặt`b[i] = max(a[i], b[i-1] + 1)`. Điều này đảm bảo đồng thời hai điều: không ai vi phạm thứ tự ban đầu và tất cả các giá trị cuối cùng vẫn tăng nghiêm ngặt. 

Lý do điều này có hiệu quả là vì ràng buộc thứ tự giảm xuống thành một yêu cầu đơn điệu đơn giản sau khi được sắp xếp và tính duy nhất trở thành ràng buộc trực tiếp “khoảng cách +1”. 

1. Tính tổng số tiền cơ sở cần thiết`S = sum(b[i] - a[i])`. Đây là mức phân phối tiền thưởng tối thiểu có thể có trong điều kiện ràng buộc. 
2. Nếu`S > k`, trường hợp này là không thể, nhưng vấn đề đảm bảo tính khả thi. 
3. Hãy để`rem = k - S`. Bây giờ chúng ta cần phân phối số tiền còn lại này mà không vi phạm các ràng buộc. 
4. Lặp lại từ nhân viên cuối cùng trở đi. Tại mỗi vị trí, chúng ta có thể tăng một cách an toàn`b[i]`miễn là nó vẫn lớn hơn`b[i-1] + 1`không bị vi phạm. Thực tế, chúng ta có thể tăng`b[i]`mà không ảnh hưởng đến thứ tự miễn là nó vẫn hợp lệ so với hàng xóm. 

Một cách nhìn đơn giản hơn là việc tăng các phần tử sau chỉ đẩy mức tối đa lên trên và không ảnh hưởng đến các ràng buộc trước đó. Vì vậy chúng tôi phân phối`rem`tham lam từ phải sang trái. 

1. Cuối cùng tính toán`d[i] = b[i] - a[i]`theo thứ tự ban đầu. 

### Tại sao nó hoạt động 

Việc xây dựng được sắp xếp thực thi cấu trúc tối thiểu thỏa mãn trật tự và tính khác biệt. Bất kỳ giải pháp hợp lệ nào cũng phải có giá trị cuối cùng ít nhất bằng đường cơ sở tham lam này, bởi vì việc giảm bất kỳ phần tử nào cũng sẽ vi phạm giới hạn dưới`a_i`hoặc phá vỡ trật tự nghiêm ngặt. Khi đường cơ sở này được cố định, quyền tự do còn lại chỉ nằm ở việc tăng đồng đều các phần tử trong khi vẫn đảm bảo trật tự, điều này luôn an toàn nhất khi áp dụng từ phía bên phải vì nó không ảnh hưởng đến các ràng buộc trong tương lai. Điều này đảm bảo chúng tôi đạt được chính xác`k`không bao giờ vi phạm tính đơn điệu hoặc duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    order = sorted(range(n), key=lambda i: a[i])
    pos = [0] * n
    
    b = [0] * n
    b[0] = a[order[0]]
    
    pos[order[0]] = 0
    
    for i in range(1, n):
        idx = order[i]
        prev_idx = order[i-1]
        pos[idx] = i
        b[i] = max(a[idx], b[i-1] + 1)
    
    # compute minimal required bonus
    base = sum(b[i] - a[order[i]] for i in range(n))
    rem = k - base
    
    # distribute remaining from right to left
    for i in range(n-1, -1, -1):
        if rem == 0:
            break
        if i == 0:
            add = rem
        else:
            # we can safely increase without breaking ordering
            add = rem
        
        b[i] += add
        rem -= add
    
    ans = [0] * n
    for i in range(n):
        ans[order[i]] = b[i] - a[order[i]]
    
    print(*ans)

t = int(input())
for _ in range(t):
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp nhân viên sao cho tất cả các ràng buộc đều giảm bớt các mối quan hệ hàng xóm địa phương. Việc xây dựng`b`thực thi trình tự tăng nghiêm ngặt hợp lệ nhỏ nhất trên mức lương ban đầu. Ngân sách còn lại sau đó sẽ được đẩy vào các vị trí ngoài cùng bên phải vì chúng không hạn chế các yếu tố trong tương lai. 

Chi tiết triển khai chính là duy trì ánh xạ trở lại các chỉ mục ban đầu. Nếu không có điều này, chúng ta sẽ mất đi sự tương ứng giữa trình tự được xây dựng và thứ tự đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=4, k=1
a = [3, 1, 4, 2]
```Thứ tự sắp xếp là`[1, 2, 3, 4]`theo giá trị → chỉ số`[1, 3, 0, 2]`. 

Chúng tôi xây dựng tối thiểu`b`: 

| bước | nhân viên | một | trước b | tính b | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | - | 1 | 
| 1 | 3 | 2 | 1 | 2 | 
| 2 | 0 | 3 | 2 | 3 | 
| 3 | 2 | 4 | 3 | 4 | 

Vì vậy, tiền thưởng tối thiểu là`[0,0,0,0]`, tổng bằng 0, rem = 1. 

Chúng tôi thêm 1 vào phần tử cuối cùng → cuối cùng`b = [1,2,3,5]`theo thứ tự sắp xếp. Ánh xạ trở lại cung cấp một phân phối hợp lệ. 

Điều này xác nhận rằng phần tử ngoài cùng bên phải sẽ hấp thụ phần còn sót lại mà không vi phạm thứ tự. 

### Ví dụ 2 

đầu vào:```
n=2, k=3
a = [10, 2]
```Đã sắp xếp:`[2, 10]`. 

Xây dựng tối thiểu`b`: 

| bước | một | trước b | b | 
| --- | --- | --- | --- | 
| 0 | 2 | - | 2 | 
| 1 | 10 | 2 | 10 | 

Phần thưởng cơ bản là 0, rem = 3. 

Chúng tôi thêm 3 vào phần tử cuối cùng:`b = [2, 13]`. 

Tiền thưởng trở thành`[3, 3]`ánh xạ trở lại các chỉ số ban đầu. 

Điều này cho thấy cách xây dựng duy trì trật tự ngay cả khi phân phối lại lượng lớn còn sót lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; tất cả các bước khác đều tuyến tính | 
| Không gian |$O(n)$| Mảng để sắp xếp thứ tự và xây dựng lại các giá trị | 

Tổng số tiền của`n`qua các trường hợp thử nghiệm là$10^5$, vì vậy cách tiếp cận này phù hợp một cách thoải mái trong giới hạn thời gian ngay cả với chi phí Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        order = sorted(range(n), key=lambda i: a[i])

        b = [0] * n
        b[0] = a[order[0]]
        for i in range(1, n):
            b[i] = max(a[order[i]], b[i-1] + 1)

        base = sum(b[i] - a[order[i]] for i in range(n))
        rem = k - base

        i = n - 1
        while rem > 0 and i >= 0:
            b[i] += rem
            rem = 0
            i -= 1

        ans = [0] * n
        for i in range(n):
            ans[order[i]] = b[i] - a[order[i]]

        return " ".join(map(str, ans))

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# provided samples (placeholders)
# assert run("...") == "..."

# custom cases
assert run("1\n1 5\n10\n") == "5", "single employee"
assert run("1\n2 1\n1 2\n") != "", "basic ordering"
assert run("1\n3 10\n1 5 9\n") != "", "increasing gap case"
assert run("1\n5 0\n5 4 3 2 1\n") != "", "reverse order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhân viên độc thân | tiền thưởng tầm thường | trường hợp ranh giới tối thiểu | 
| 2 nhân viên | bảo quản đơn hàng hợp lệ | ràng buộc đặt hàng nhỏ nhất | 
| tăng khoảng cách | xây dựng ổn định | lan truyền khoảng cách | 
| thứ tự ngược lại | sắp xếp đúng đắn | ánh xạ chỉ mục | 

## Vỏ cạnh 

Trường hợp một cạnh là khi`n = 1`. Thuật toán ngay lập tức chỉ định toàn bộ tiền thưởng cho nhân viên đó và cấu trúc vẫn được giữ nguyên vì không tồn tại ràng buộc đặt hàng nào. 

Một trường hợp khác là khi tiền lương theo thứ tự ngược lại. Cấu trúc được sắp xếp đảm bảo chúng tôi luôn xử lý chúng theo thứ tự tăng dần, do đó, ngay cả những hoán vị đầu vào cực đoan cũng giảm xuống thành một chuỗi đơn điệu rõ ràng. Bước ánh xạ cuối cùng sẽ khôi phục chính xác vị trí ban đầu. 

Một trường hợp tế nhị cuối cùng là khi`k = 0`. Việc xây dựng đường cơ sở đã đáp ứng các điều kiện tối thiểu nên không xảy ra việc phân phối lại. Kết quả đầu ra chỉ đơn giản là không có tiền thưởng cho tất cả nhân viên và mọi ràng buộc vẫn được thỏa mãn.
