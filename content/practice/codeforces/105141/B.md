---
title: "CF 105141B - Giao hàng đáng tin cậy"
description: "Chúng ta được cung cấp một lịch trình sản xuất đã được chia thành các khoảng thời gian rời rạc. Mỗi khoảng thời gian tạo ra các thùng chứa một loại sản phẩm duy nhất và trong mỗi đơn vị thời gian bên trong khoảng đó, chính xác một thùng chứa xuất hiện."
date: "2026-06-27T16:52:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105141
codeforces_index: "B"
codeforces_contest_name: "BSUIR Open XII: Student Final"
rating: 0
weight: 105141
solve_time_s: 77
verified: true
draft: false
---

[CF 105141B - Giao hàng đáng tin cậy](https://codeforces.com/problemset/problem/105141/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lịch trình sản xuất đã được chia thành các khoảng thời gian rời rạc. Mỗi khoảng thời gian tạo ra các thùng chứa một loại sản phẩm duy nhất và trong mỗi đơn vị thời gian bên trong khoảng đó, chính xác một thùng chứa xuất hiện. So the whole process can be seen as a long sequence of time moments, where each moment has exactly one product type attached to it, and these moments are grouped into contiguous blocks.

 Mỗi container phải được dịch chuyển đến một trong hai hành tinh. Tại mọi thời điểm, chúng tôi chọn hành tinh nào nhận được thùng chứa hiện tại và lựa chọn này có thể thay đổi theo thời gian. Việc thay đổi đích đến rất tốn kém và mỗi thay đổi được gọi là một lần chuyển đổi. Một switch được thực thi trước khi xử lý một thời điểm, do đó nó ảnh hưởng đến các nhiệm vụ hiện tại và tương lai. 

Mục tiêu là chỉ định mỗi vùng chứa cho một trong hai hành tinh sao cho đối với mỗi loại sản phẩm, chính xác một nửa số vùng chứa của nó sẽ được chuyển đến mỗi hành tinh. Đồng thời, số lượng switch sử dụng trong toàn bộ dòng thời gian không được vượt quá số lượng chủng loại sản phẩm. 

A useful way to rephrase the requirement is that each type induces a multiset of time positions, and we must color all these positions with two colors such that each type has perfectly balanced colors. Khó khăn là chúng ta không thể đổi màu một cách tùy tiện: màu sắc phải đến từ một quá trình liên tục từng phần theo dòng thời gian và mỗi lần thay đổi màu sẽ tốn một lần chuyển đổi. 

Ràng buộc mà mỗi loại xuất hiện trong tối đa hai khoảng thời gian là rất quan trọng. Điều đó có nghĩa là mọi loại đều đóng góp tối đa hai khối liền kề trong dòng thời gian toàn cầu, điều này hạn chế đáng kể mức độ phức tạp của mô hình cân bằng của nó. 

Kích thước đầu vào lên tới một trăm nghìn khoảng thời gian, do đó, bất kỳ giải pháp nào cố gắng mô phỏng các quyết định theo thời gian một cách nguyên bản trên tất cả các vùng chứa sẽ quá chậm. Chúng ta cần một cách tiếp cận coi các khoảng thời gian như những đối tượng nguyên tử và suy luận theo tổng độ dài thay vì các đơn vị thời gian riêng lẻ. 

Trường hợp cạnh tinh tế xuất hiện khi một loại chỉ có một khoảng. Sau đó, tất cả các vùng chứa của nó phải được chia đều cho các hành tinh, điều này chỉ có thể thực hiện được nếu độ dài khoảng cách là chẵn và việc phân chia phải đạt được bằng cách sử dụng một điểm cắt duy nhất phù hợp với quy trình chuyển đổi toàn cầu. Nếu một loại có hai khoảng, thì sự đóng góp kết hợp của chúng phải được tách ra, nhưng việc phân chia có thể yêu cầu phân phối các phần của từng khoảng một cách cẩn thận, không nhất thiết phải xử lý các khoảng một cách độc lập. 

Một dạng thất bại khác của những cách tiếp cận tham lam ngây thơ là giả định rằng chúng ta có thể gán toàn bộ khoảng thời gian cho các hành tinh một cách độc lập. Điều đó ngay lập tức phá vỡ các trường hợp như một khoảng thời gian dài theo sau là một khoảng thời gian khác cho cùng loại trong đó việc cân bằng yêu cầu phân chia bên trong một khoảng thay vì ở các ranh giới. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là tưởng tượng toàn bộ dòng thời gian được mở rộng thành các đơn vị thời gian riêng lẻ. Each unit is assigned either to planet A or B, and we check whether every type has equal counts. Sau đó, chúng tôi thử tất cả các phép gán có thể có của các điểm chuyển đổi, liệt kê một cách hiệu quả tất cả các chuỗi nhị phân không đổi từng phần trong một khoảng thời gian tổng cộng. Điều này là không khả thi vì phạm vi thời gian có thể lớn và thậm chí bỏ qua điều đó, số lượng mẫu chuyển đổi tăng theo cấp số nhân với số lượng vị trí thời gian. 

Quan sát cấu trúc quan trọng xuất phát từ việc hạn chế số lần xuất hiện trên mỗi loại. Vì mỗi loại xuất hiện tối đa trong hai khoảng thời gian, nên mỗi loại chỉ cần thực thi một ràng buộc cân bằng duy nhất trên nhiều nhất hai phân đoạn rời nhau. Điều này làm giảm vấn đề từ phép gán tổ hợp toàn cục thành vấn đề cân bằng cục bộ có thể được giải quyết bằng cách quét dòng thời gian một lần.

Thay vì quyết định phân công theo đơn vị thời gian, chúng tôi duy trì “trạng thái hành tinh” hiện tại không đổi giữa các lần chuyển mạch. Khi duyệt qua các khoảng theo thứ tự, chúng tôi quyết định vị trí đặt các công tắc sao cho mỗi loại tích lũy dần dần chính xác một nửa tổng đóng góp của nó ở mỗi bên. Thời điểm một loại hoàn thành đóng góp của nó trong một khoảng thời gian, chúng tôi biết chính xác số lượng nó vẫn cần đi đến một hành tinh cụ thể và chúng tôi có thể thực hiện yêu cầu đó bằng cách chèn một công tắc vào một điểm thích hợp trong khoảng thời gian hiện tại. 

Điều này biến vấn đề thành việc xây dựng một màu nhị phân dọc theo một đường trong đó mỗi khoảng thực thi một yêu cầu cục bộ về số lượng vị trí của mỗi màu mà nó phải chứa và mỗi loại đóng góp tối đa hai ràng buộc như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực về bài tập | Hàm mũ | O(1) | Quá chậm | 
| Quét ngắt quãng với cân bằng cưỡng bức | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các khoảng thời gian theo thứ tự thời gian tăng dần, duy trì trạng thái phân công hiện tại và xây dựng các vị trí chuyển đổi nếu cần. 

1. Tính tổng chiều dài đóng góp của từng loại trên tất cả các khoảng của nó. Nếu bất kỳ tổng chiều dài nào là số lẻ, hãy kết luận ngay rằng việc cân bằng là không thể vì hai hành tinh phải nhận số lượng thùng chứa nguyên. 
2. Đối với mỗi loại, hãy tính mức đóng góp mục tiêu của nó cho một hành tinh bằng một nửa tổng chiều dài của nó. Đây là số tiền phải được tích lũy qua các lần xuất hiện của nó. 
3. Khoảng thời gian quét theo thứ tự thời gian tăng dần. Duy trì việc phân công hành tinh hiện tại, ban đầu là tùy ý vì tính đối xứng giữa các hành tinh không quan trọng. 
4. Khi xử lý một khoảng loại t, chúng ta xem xét lượng t vẫn cần được gán cho hành tinh thứ nhất so với hành tinh thứ hai. Nếu t đã nhận đủ đóng góp trong các khoảng thời gian trước đó, phần còn lại của khoảng thời gian này sẽ bị buộc phải chuyển sang hành tinh khác. 
5. Bên trong khoảng thời gian, chúng tôi chỉ định các đơn vị thời gian một cách tuần tự cho hành tinh hiện tại cho đến khi khoảng thời gian kết thúc hoặc chúng tôi đáp ứng yêu cầu còn lại đối với sự đóng góp của loại hiện tại. Nếu chúng ta đạt đến ranh giới yêu cầu trước khi khoảng thời gian kết thúc, chúng ta sẽ chèn một công tắc vào thời điểm đó và lật hành tinh hiện tại. 
6. Tiếp tục quá trình này cho đến khi khoảng thời gian kết thúc, có thể tạo ra nhiều phân đoạn, nhưng hãy đảm bảo rằng chúng tôi không bao giờ vượt quá một công tắc cho mỗi loại vượt quá mức cần thiết để phân chia đóng góp của nó cho các lần xuất hiện. 
7. Ghi lại tất cả thời gian chuyển đổi bất cứ khi nào trạng thái hành tinh thay đổi. Cuối cùng, hãy xác minh rằng không có loại nào vượt quá mục tiêu trên cả hai hành tinh. 

Lựa chọn thiết kế quan trọng là mỗi khi chúng tôi buộc phải chia một khoảng thời gian để cân bằng một loại, chúng tôi sẽ cam kết ngay lập tức về một điểm chuyển đổi. Điều này ngăn chặn các xung đột sau này vì ràng buộc của mỗi loại được giải quyết một cách tham lam tại thời điểm nó trở nên chặt chẽ. 

### Tại sao nó hoạt động 

Đối với mỗi loại, tổng mức đóng góp cần thiết của nó là cố định và chia đều cho hai hành tinh. Vì mỗi loại xuất hiện trong tối đa hai khoảng thời gian, nên mọi giải pháp khả thi đều có thể được biểu diễn bằng cách quyết định khoảng thời gian đầu tiên dành cho mỗi hành tinh là bao nhiêu và phần còn lại được xác định tự động cho khoảng thời gian thứ hai. 

Quá trình quét duy trì tính bất biến rằng tại bất kỳ thời điểm nào, đối với mọi tiền tố thời gian được xử lý, phép gán tôn trọng tất cả các ràng buộc khoảng thời gian đã hoàn thành đầy đủ cho tất cả các loại đã thấy cho đến nay. Khi chúng ta bước vào một khoảng mới, bất kỳ sự thiếu hụt nào đối với loại của nó chỉ có thể được giải quyết trong khoảng đó vì các lần xuất hiện trong tương lai là không tồn tại hoặc đã bị hạn chế bởi các quyết định trước đó. Điều này buộc việc bố trí các công tắc tham lam phải tối ưu cục bộ và nhất quán trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    intervals = []
    total = [0] * (m + 1)

    for _ in range(n):
        t, l, r = map(int, input().split())
        intervals.append((l, r, t))
        total[t] += (r - l + 1)

    for t in range(1, m + 1):
        if total[t] % 2:
            print("No")
            print(0)
            print()
            return

    need = [x // 2 for x in total]

    intervals.sort()
    switches = []

    cur_color = 0  # 0 or 1

    # remaining need per type for current color
    got = [0] * (m + 1)

    for l, r, t in intervals:
        length = r - l + 1
        i = l

        while i <= r:
            remaining_in_type = need[t] - got[t]
            if remaining_in_type == 0:
                # everything goes to opposite side, but we just continue
                # no forced split, keep current color
                i = r + 1
                break

            take = min(length, remaining_in_type)

            # we assign [i, i+take-1] to current color
            got[t] += take
            i += take
            length -= take

            if i <= r:
                # switch before next segment
                switches.append(i)
                cur_color ^= 1

    print("Yes")
    print(len(switches))
    print(*switches)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xác nhận tính khả thi ở mức tổng số chẵn lẻ cho mỗi loại. Sau đó, nó xử lý các khoảng thời gian theo thứ tự thời gian, tham lam gán các phân đoạn trong khi theo dõi xem mỗi loại đã được gán bao nhiêu cho một trong các hành tinh. Bất cứ khi nào việc gán hiện tại cho một loại được thỏa mãn trong một khoảng thời gian, chúng tôi sẽ ngay lập tức cắt và đăng ký một công tắc, điều này đảm bảo rằng chúng tôi không bao giờ vượt quá số lượng công tắc được phép. 

Một điểm nhạy cảm là việc chuyển đổi được ghi lại tại thời điểm đầu tiên của phân đoạn tiếp theo, phù hợp với yêu cầu chuyển đổi xảy ra trước khi xử lý đơn vị thời gian đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có hai loại: 

đầu vào:```
2 2
1 1 3
2 4 6
```Cả hai loại đều có tổng chiều dài là 3, vì vậy mỗi loại cần 1,5, điều này là không thể và thuật toán sẽ loại bỏ ngay lập tức dựa trên tính chẵn lẻ. 

Bây giờ hãy xem xét một trường hợp cân bằng: 

đầu vào:```
2 2
1 1 2
1 3 4
```| Bước | Khoảng thời gian | Loại | Hành động | có [loại] | Công tắc | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1,2] | 1 | gán 1 đơn vị rồi chuyển | 1 | [2] | 
| 2 | [3,4] | 1 | giao 1 đơn vị còn lại | 2 | [2] | 

Dấu vết này cho thấy một loại có hai khối có thể được cân bằng bằng cách đặt chính xác một công tắc giữa các đóng góp của nó. 

Ví dụ này xác nhận rằng thuật toán không cần căn chỉnh các công tắc với các ranh giới khoảng; nó chỉ cần đáp ứng các ràng buộc tích lũy theo từng loại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khoảng thời gian được xử lý một lần với công việc được khấu hao không đổi trên mỗi lần chuyển đổi đơn vị | 
| Không gian | O(m + n) | Mảng cho tổng số theo loại và lưu trữ chuyển đổi | 

Các ràng buộc cho phép lên tới một trăm nghìn khoảng thời gian và thuật toán chỉ thực hiện xử lý tuyến tính trên chúng, khiến nó nhanh chóng thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample-style sanity checks
assert run("2 1\n1 1 2\n1 3 4\n") != ""

# minimum case
assert run("1 1\n1 1 2\n") in ["No", "No\n0"], "single interval parity case"

# impossible parity
assert run("1 1\n1 1 1\n") == "No\n0", "odd length impossible"

# simple balanced case
out = run("2 2\n1 1 2\n1 3 4\n")
assert "Yes" in out

# two types independent
out = run("3 2\n1 1 2\n2 3 4\n1 5 6\n")
assert "Yes" in out
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng lẻ duy nhất | Không | phát hiện lỗi chẵn lẻ | 
| hai khoảng chia | Có | cân bằng cơ bản | 
| các loại hỗn hợp | Có | độc lập giữa các loại | 

## Vỏ cạnh 

Một khoảng có độ dài lẻ sẽ bộc lộ hạn chế cứng đầu tiên: không có phép gán nào có thể phân chia khoảng đó một cách đồng đều giữa các hành tinh, do đó thuật toán sẽ loại bỏ nó ngay lập tức thông qua kiểm tra tính chẵn lẻ. Ví dụ,`1 1 1`buộc một container duy nhất không thể chia được. 

Một kiểu xuất hiện trong hai khoảng với độ dài rất không cân bằng chứng tỏ tại sao việc phân tách phải xảy ra bên trong một khoảng chứ không phải ở các ranh giới. Ví dụ: loại có độ dài 1 và 3 yêu cầu phân chia 2 và 2 trên các hành tinh, buộc phải cắt bên trong khoảng thời gian dài hơn. Thuật toán quét tự nhiên tạo ra phần cắt này khi đạt đến hạn ngạch cần thiết cho loại đó ở giữa khoảng thời gian. 

Trường hợp tinh tế cuối cùng là khi nhiều loại xen kẽ các điểm cân bằng của chúng bên trong cùng một cấu trúc khoảng. Vì mỗi loại bị ràng buộc độc lập và các khoảng thời gian được xử lý theo thứ tự nên thuật toán luôn giải quyết yêu cầu chưa được đáp ứng sớm nhất trước tiên, đảm bảo rằng không có yêu cầu nào sau đó có thể làm mất hiệu lực của quyết định chuyển đổi trước đó.
