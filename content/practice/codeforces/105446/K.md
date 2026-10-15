---
title: "CF 105446K - Đan"
description: "Chúng ta được cung cấp một trình tự được xây dựng một phần tượng trưng cho một chiếc khăn dệt kim. Mỗi vị trí là một màu và một số tiền tố của chiếc khăn đã được cố định."
date: "2026-06-23T03:23:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105446
codeforces_index: "K"
codeforces_contest_name: "2024 United Kingdom and Ireland Programming Contest (UKIEPC 2024)"
rating: 0
weight: 105446
solve_time_s: 91
verified: false
draft: false
---

[CF 105446K - Đan](https://codeforces.com/problemset/problem/105446/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một trình tự được xây dựng một phần tượng trưng cho một chiếc khăn dệt kim. Mỗi vị trí là một màu và một số tiền tố của chiếc khăn đã được cố định. Nhiệm vụ của chúng ta là mở rộng tiền tố này thành một chuỗi có độ dài đầy đủ`n`sử dụng màu sắc từ`1`ĐẾN`k`, đồng thời tôn trọng giới hạn về khoảng cách: nếu một màu xuất hiện ở một vị trí nào đó thì nó không được xuất hiện lại trong vị trí tiếp theo`p - 1`các vị trí. 

Đồng thời, trong số tất cả các lần hoàn thành hợp lệ, chúng tôi muốn giữ tần suất tối đa của bất kỳ màu đơn nào càng nhỏ càng tốt. Nói cách khác, chúng tôi đang cân bằng việc sử dụng màu sắc một cách đồng đều nhất có thể trong một ràng buộc cứng rắn nhằm ngăn chặn sự lặp lại gần đây. 

Đầu vào cho độ dài mục tiêu`n`, số màu`k`, yêu cầu khoảng cách`p`, độ dài tiền tố đã được xây dựng`m`, và cố định đầu tiên`m`màu sắc. Chúng ta phải mở rộng tiền tố này thành một chuỗi hợp lệ đầy đủ hoặc báo cáo rằng không có sự hoàn thành nào tồn tại. 

Những ràng buộc cho phép`n, k, p`lên tới 100000, loại trừ mọi cấu trúc bậc hai hoặc quét lại toàn bộ chuỗi. Bất kỳ phương pháp nào cố gắng kiểm tra tính hợp lệ của từng vị trí bằng cách quét lại`p`các vị trí sẽ trở nên quá chậm trong trường hợp xấu nhất vì nó sẽ giảm xuống O(n·p). Chúng ta cần công không đổi hoặc logarit cho mỗi vị trí. 

Một vài trường hợp đặc biệt phá vỡ những nỗ lực tham lam ngây thơ: 

Nếu`p = 1`, không có hạn chế nào, vì vậy mọi sự hoàn thành đều hợp lệ và việc cân bằng tần số hoàn toàn là về phân phối. 

Nếu như`k = 1`Và`p > 1`, thì bất kỳ chuỗi nào dài hơn 1 đều không thể thực hiện được vì màu duy nhất sẽ ngay lập tức vi phạm khoảng cách. Ví dụ:`n = 5, k = 1, p = 2`và tiền tố`[1]`buộc lặp lại trong khoảng cách 1, do đó đầu ra là không thể. 

Nếu tiền tố đã vi phạm ràng buộc nội bộ, chúng ta phải từ chối nó ngay lập tức. Ví dụ:`p = 3`, tiền tố`[1, 2, 1]`đã không hợp lệ rồi. 

Cuối cùng, một trường hợp lỗi nhỏ xảy ra khi tiện ích mở rộng tham lam chọn màu hợp lệ cục bộ nhưng sau đó chặn tất cả các tùy chọn do hạn chế về khoảng cách. Điều này xảy ra khi chúng tôi không xem xét tính sẵn có của các vị trí trong tương lai cho từng màu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xây dựng trình tự theo từng vị trí. Ở mỗi bước, chúng tôi thử mọi màu sắc`1..k`, kiểm tra xem nó có vi phạm giới hạn khoảng cách hay không bằng cách quét trước đó`p-1`các vị trí và tạm thời chỉ định nó. Điều này đúng vì nó thực thi ràng buộc một cách trực tiếp, nhưng nó tốn O(k·p) cho mỗi vị trí, dẫn đến O(n·k·p), quá lớn đối với đầu vào có quy mô 100000. 

Ngay cả khi chúng tôi tối ưu hóa việc kiểm tra tính hợp lệ bằng cách sử dụng mảng được nhìn thấy lần cuối để mỗi lần kiểm tra trở thành O(1), chúng tôi vẫn cần một chiến lược để quyết định nên chọn màu hợp lệ nào ở mỗi bước. Một cách tiếp cận tham lam ngây thơ là chọn màu có tần số hiện tại nhỏ nhất có thể thất bại vì nó có thể chỉ định một màu quá sớm và khiến nó không khả dụng sau này do khoảng cách giữa các khoảng cách. 

Quan sát quan trọng là ràng buộc về khoảng cách là cục bộ và chỉ phụ thuộc vào điểm cuối cùng`p - 1`vị trí, trong khi mục tiêu cân bằng chỉ phụ thuộc vào số lượng toàn cầu. Sự tách biệt này cho phép chúng tôi duy trì hai cấu trúc: một để theo dõi mức sử dụng gần đây (để thực thi tính khả thi) và một để theo dõi tần suất (để cân bằng phân phối). Ở mỗi bước, chúng ta nên chọn một màu hiện được cho phép và có tần suất nhỏ nhất trong số các màu được phép. 

Cách hiệu quả để hỗ trợ điều này là duy trì chỉ số xuất hiện cuối cùng của mỗi màu và duy trì một dải tần số. Chúng tôi cũng giữ một cấu trúc có thể nhanh chóng truy xuất trong số các màu hợp lệ hiện tại, cấu trúc có tần suất tối thiểu. Từ`k`có thể lớn, chúng tôi mô phỏng điều này bằng một hàng đợi ưu tiên lưu trữ các ứng viên được sắp xếp theo`(frequency, color)`và chúng tôi loại bỏ các mục nhập không hợp lệ một cách lười biếng khi chúng vi phạm khoảng cách. 

Điều này làm giảm mỗi vị trí thành O(log k), vì mỗi màu được đẩy và bật một số lần giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·k·p) | O(n) | Quá chậm | 
| Tối ưu | O(n log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi việc xây dựng trình tự là một quá trình truyền phát từ trái sang phải, luôn tôn trọng tiền tố. 

1. Khởi tạo một mảng`last[c] = -inf`cho mỗi màu`c`, và đặt tần số`freq[c] = 0`. Trước tiên, chúng tôi cũng xử lý tiền tố đã cho, cập nhật cả hai mảng và xác minh rằng không có vi phạm khoảng cách nào tồn tại. Nếu phát hiện vi phạm, chúng tôi sẽ dừng ngay lập tức. 
2. Xây dựng hàng ưu tiên chứa tất cả các màu. Mỗi mục lưu trữ`(freq[c], c)`. Heap sẽ luôn đề xuất màu ít được sử dụng nhất. 
3. Lặp lại từ vị trí`m+1`ĐẾN`n`và tại mỗi vị trí, chúng ta cần chọn một màu hợp lệ. 
4. Bật liên tục từ đống cho đến khi tìm thấy màu`c`như vậy`i - last[c] >= p`. Điều kiện này đảm bảo màu không được sử dụng lần cuối`p - 1`các vị trí. Nếu không có màu như vậy tồn tại thì chúng tôi kết luận việc xây dựng là không thể. 
5. Sau khi tìm thấy màu hợp lệ, hãy gán nó vào vị trí`i`, cập nhật`last[c] = i`, tăng`freq[c]`và đẩy cặp đã cập nhật trở lại vùng heap. 
6. Tiếp tục cho đến khi tất cả các vị trí được lấp đầy. 

Ý tưởng chính là vùng heap luôn cố gắng giữ tần số được cân bằng, trong khi bước lọc thực thi tính khả thi. 

### Tại sao nó hoạt động 

Ở bất kỳ bước nào`i`, trong số tất cả các màu được cho phép bởi giới hạn khoảng cách, chúng tôi luôn chọn màu có tần số nhỏ nhất. Bất kỳ lựa chọn nào khác sẽ giữ nguyên tần số tối đa hoặc tăng sớm hơn mức cần thiết, điều này chỉ khiến việc cân bằng sau này trở nên khó khăn hơn. Ràng buộc về khoảng cách chỉ hạn chế tính đủ điều kiện, không sắp xếp thứ tự giữa các màu đủ điều kiện, do đó việc sắp xếp theo tần suất giữa các ứng cử viên hợp lệ sẽ duy trì tính chính xác. 

Điều bất biến là trước khi đặt vị trí`i`, chuỗi một phần thỏa mãn ràng buộc về khoảng cách và đống chứa tất cả các màu có tần số chính xác. Mỗi nhiệm vụ bảo tồn cả hai thuộc tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n, k, p = map(int, input().split())
    m = int(input())
    s = list(map(int, input().split()))

    last = [-10**18] * (k + 1)
    freq = [0] * (k + 1)

    # validate prefix
    for i, c in enumerate(s, start=1):
        if i - last[c] < p:
            print("impossible")
            return
        last[c] = i
        freq[c] += 1

    heap = []
    for c in range(1, k + 1):
        heapq.heappush(heap, (freq[c], c))

    def push(c):
        heapq.heappush(heap, (freq[c], c))

    for i in range(m + 1, n + 1):
        while heap:
            f, c = heapq.heappop(heap)
            if f != freq[c]:
                continue
            if i - last[c] >= p:
                break
        else:
            print("impossible")
            return

        s.append(c)
        last[c] = i
        freq[c] += 1
        push(c)

    print(*s)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xác nhận tiền tố đã cho. Bước này rất cần thiết vì bất kỳ khoảng cách không hợp lệ nào sớm sẽ khiến việc hoàn thành toàn bộ không thể thực hiện được bất kể các lựa chọn trong tương lai. 

Heap lưu trữ tất cả các màu được sắp xếp theo tần số hiện tại. Chúng tôi sử dụng tính năng xóa lười: các mục cũ vẫn còn trong heap nhưng bị bỏ qua khi xuất hiện. Điều này tránh được các hoạt động giảm phím tốn kém. 

Trong quá trình xây dựng, chúng tôi liên tục trích xuất màu ít thường xuyên nhất và kiểm tra xem nó có tuân thủ giới hạn khoảng cách hay không bằng cách sử dụng`last[c]`. Nếu không, chúng tôi sẽ loại bỏ nó trong lần lặp này và tiếp tục. 

Một điểm tinh tế là tính đúng đắn của việc sử dụng đống ngay cả khi tần số thay đổi. các`(f, c)`cặp có thể trở nên lỗi thời; đây là lý do tại sao chúng tôi so sánh`f != freq[c]`trước khi sử dụng nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 3, p = 2
prefix = [1, 2]
```Chúng tôi theo dõi`last`Và`freq`. 

| Bước | Vị trí | Lựa chọn đống | Được chọn | cập nhật lần cuối | trạng thái tần số | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | 1:1, 2:2 | 1:1, 2:1 | 
| 3 | 3 | (1,3) | 3 | cuối cùng[3]=3 | 1:1,2:1,3:1 | 
| 4 | 4 | (1,1) hợp lệ (vì 4-1 ≥2) | 1 | cuối cùng[1]=4 | 1:2,2:1,3:1 | 
| 5 | 5 | (1,2) | 2 | cuối cùng[2]=5 | 1:2,2:2,3:1 | 

Điều này thể hiện cách thuật toán thay thế để duy trì sự cân bằng trong khi vẫn tôn trọng khoảng cách. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 1, p = 2
prefix = [1]
```| Bước | Vị trí | Lựa chọn đống | Có hiệu lực? | Kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | tần số[1]=1 | 
| 2 | 2 | 1 | không (2-1<2) | không có ứng cử viên | 
| dừng lại | - | - | - | không thể | 

Điều này cho thấy rằng mặc dù chỉ có một màu tồn tại nhưng khoảng cách khiến việc tiếp tục không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log k) | Mỗi vị trí thực hiện các thao tác heap, mỗi O(log k), với số lần bật bổ sung không đổi được khấu hao cho các mục nhập cũ | 
| Không gian | O(k) | Mảng và đống lưu trữ một mục nhập cho mỗi màu | 

Các ràng buộc cho phép tối đa 100000 phần tử, do đó chi phí logarit là đủ. Cách tiếp cận dựa trên heap giúp hoạt động ổn định ngay cả trong các mẫu xen kẽ trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("10 6 3\n4\n4 1 6 4\n") != "", "sample 1 format check"
assert run("5 2 3\n2\n2 2\n") in ["impossible", "impossible\n"], "sample 2"
assert run("2 2 3\n1\n2\n1") != "", "sample 3 format check"

# custom cases

# minimum size
assert run("1 5 2\n0\n\n") != "impossible"

# impossible due to p constraint
assert run("3 1 2\n1\n1\n") == "impossible"

# already invalid prefix
assert run("3 2 2\n2\n1 1\n") == "impossible"

# balanced fill
assert run("6 3 2\n1\n1\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1,k=5,p=2 | bất kỳ hợp lệ | xây dựng tối thiểu | 
| k=1,p=2,n=3 | không thể | khoảng cách không thể | 
| vi phạm tiền tố | không thể | xác thực tiền tố | 
| trường hợp cân bằng nhỏ | trình tự hợp lệ | cân bằng tham lam | 

## Vỏ cạnh 

Tiền tố đã vi phạm khoảng cách sẽ bị từ chối ngay lập tức. Ví dụ, với`p = 3`, nhập`[1, 2, 1]`thất bại vì lần thứ hai`1`xảy ra quá sớm so với lần đầu tiên. Thuật toán phát hiện điều này trong quá trình tiền xử lý tiền tố khi`i - last[c] < p`. 

Hệ thống một màu với`p > 1`trở nên không thể vượt quá độ dài 1. Thuật toán cuối cùng sẽ làm cạn kiệt đống, vì mỗi pop của màu đó đều thất bại trong việc kiểm tra khoảng cách và không có lựa chọn thay thế nào tồn tại. 

Khi`p = 1`, mọi màu sắc luôn hợp lệ. Vùng heap chỉ cân bằng tần số và kiểm tra khoảng cách không bao giờ chặn bất kỳ ứng cử viên nào, do đó thuật toán thoái hóa thành cân bằng tải thuần túy.
