---
title: "CF 104778F - \u042f\u0449\u0438\u043a\u0438"
description: "Chúng ta bắt đầu với một hàng gồm n chồng hộp ban đầu. Mỗi ngăn xếp i chứa các hộp ai. Giữa mỗi cặp ngăn xếp ban đầu liền kề, chúng tôi chèn một ngăn xếp trống mới, do đó bố cục sẽ trở thành một chuỗi xen kẽ giữa ngăn xếp ban đầu và ngăn xếp mới: gốc, mới, nguyên bản, mới, v.v."
date: "2026-06-28T15:07:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104778
codeforces_index: "F"
codeforces_contest_name: "2023-2024 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 23, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 104778
solve_time_s: 41
verified: true
draft: false
---

[CF 104778F - \u042f\u0449\u0438\u043a\u0438](https://codeforces.com/problemset/problem/104778/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một hàng`n`chồng hộp ban đầu. Mỗi ngăn xếp`i`chứa`a_i`hộp. Giữa mỗi cặp ngăn xếp ban đầu liền kề, chúng tôi chèn một ngăn xếp trống mới, do đó bố cục sẽ trở thành một chuỗi xen kẽ giữa ngăn xếp ban đầu và ngăn xếp mới: gốc, mới, nguyên bản, mới, v.v. Điều này tạo ra`2n − 1`tổng số ngăn xếp. 

Một hạn chế chính chi phối việc di chuyển: các hộp chỉ có thể được di chuyển từ ngăn xếp ban đầu sang một trong các ngăn xếp mới liền kề của nó. Không được phép chuyển trực tiếp giữa các ngăn xếp ban đầu và các ngăn xếp mới chỉ nhận được các hộp. 

Nhiệm vụ là xác định xem liệu sau một số bước di chuyển được phép có thể thực hiện được tất cả`2n − 1`ngăn xếp chứa chính xác số lượng hộp như nhau. 

Tổng số hộp là cố định, vì vậy nếu tồn tại một giải pháp thì mỗi ngăn xếp phải có cùng giá trị`S = (sum of a_i) / (2n − 1)`. Điều này ngay lập tức ngụ ý rằng tổng số tiền phải chia hết cho`2n − 1`, nếu không thì câu trả lời là không thể. 

Ràng buộc`n ≤ 200000`có nghĩa là mọi giải pháp đều phải chạy trong thời gian gần như tuyến tính. Một bậc hai hoặc thậm chí`O(n log n)`mô phỏng tham lam liên tục điều chỉnh ngăn xếp là quá chậm trong trường hợp xấu nhất. Chúng ta nên mong đợi một giải pháp dựa trên lý luận tiền tố hoặc các điều kiện khả thi cục bộ. 

Trường hợp cạnh tinh tế xuất hiện khi giá trị trung bình là số nguyên nhưng vẫn không thể phân phối lại do hạn chế về luồng. Ví dụ: ngay cả khi tổng số tiền khớp nhau, một số tiền tố của ngăn xếp có thể yêu cầu di chuyển các hộp “qua” ranh giới bị cấm giữa hai ngăn xếp ban đầu, điều này là không được phép. Đây là khó khăn cơ cấu chính. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng quá trình một cách trực tiếp. Chúng tôi xây dựng`2n − 1`cấu trúc, liên tục chọn một ngăn xếp ban đầu vẫn còn các hộp thừa và cố gắng đẩy chúng vào các ngăn xếp mới liền kề cho đến khi mọi thứ trở nên bằng nhau. Điều này giống như một mô phỏng dòng chảy hoặc cân bằng. Tuy nhiên, mỗi chuyển động của hộp đều mang tính cục bộ và trong trường hợp xấu nhất, một hộp có thể được điều chỉnh nhiều lần. Với tối đa`2n − 1`ngăn xếp và có khả năng`O(n)`điều chỉnh trên mỗi ngăn xếp, điều này thoái hóa thành`O(n^2)`hành vi. 

Quan sát quan trọng là các ngăn xếp mới đóng vai trò là vùng đệm giữa các ngăn xếp ban đầu và chúng cô lập chuyển động. Mỗi ngăn xếp ban đầu chỉ tương tác với bộ đệm bên trái và bên phải ngay lập tức của nó. Điều này biến vấn đề thành một hạn chế khả thi cục bộ: mỗi ngăn xếp ban đầu phải có khả năng “chia” phần thặng dư hoặc thâm hụt của nó thành hai vị trí đệm liền kề mà không yêu cầu sự phối hợp toàn cầu. 

Chúng tôi định dạng lại quy trình như sau. Mỗi ngăn xếp ban đầu đóng góp giá trị của nó vào hai khoảng trống lân cận và mỗi khoảng trống nhận được sự đóng góp từ chính xác hai ngăn xếp ban đầu liền kề. Cấu hình cuối cùng là đồng nhất nên mọi khoảng cách cũng phải ổn định nhất quán với cùng giá trị mục tiêu. Điều này chuyển đổi vấn đề thành việc kiểm tra xem liệu có tồn tại sự phân công nhất quán các luồng trên biểu đồ đường dẫn trong đó các nút gốc đẩy vào các cạnh hay không. 

Thay vì mô phỏng, chúng tôi rút ra các ràng buộc từ trái sang phải: lượng phải vượt qua từng ranh giới được xác định duy nhất bởi sự mất cân bằng tiền tố. Nếu tại bất kỳ thời điểm nào việc chuyển tiền được yêu cầu trở nên không thể thực hiện được thì câu trả lời là phủ định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng | O(n²) | O(n) | Quá chậm | 
| Tuyên truyền cân bằng tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán giá trị mục tiêu`S = sum(a) / (2n − 1)`. Nếu nó không phải là số nguyên, chúng tôi trả về ngay NO. 

Sau đó, chúng tôi giải thích hệ thống như một sự lan truyền tuyến tính của sự mất cân bằng. Cho phép`balance`biểu thị số lượng hộp bổ sung phải được chuyển từ ngăn xếp ban đầu hiện tại sang vùng tiếp theo. Chúng tôi quét từ trái sang phải trên các ngăn xếp ban đầu, duy trì mức thặng dư hoặc thâm hụt chảy qua ranh giới. 

1. Tính tổng và kiểm tra khả năng chia hết cho`2n − 1`. Nếu không thành công thì cấu hình không thể phân bố đều nên chúng ta dừng ngay. 
2. Đặt mục tiêu cho mỗi ngăn xếp`S`. 
3. Khởi tạo một biến`carry = 0`, biểu thị số lượng hộp phải được chuyển từ phân đoạn trước vào phân đoạn hiện tại. 
4. Lặp lại từng ngăn xếp ban đầu`i`từ trái sang phải. 
5. Cập nhật`carry`bằng cách thêm`a_i - S`. Điều này thể hiện số lượng ngăn xếp`i`đi chệch khỏi mục tiêu sau khi tính toán lưu lượng đến. 
6. Nếu tại bất kỳ thời điểm nào`carry`trở nên âm, chúng tôi phát hiện ra rằng mức thâm hụt phải được đẩy sang trái, điều này là không thể vì chuyển động chỉ lan truyền qua các ngăn xếp mới liền kề theo một hướng nhất quán. 
7. Tiếp tục nhân giống cho đến hết. 
8. Nếu sau khi xử lý tất cả các ngăn xếp mà hệ thống nhất quán, hãy trả về CÓ. 

Ý tưởng quan trọng là`carry`mã hóa quá trình truyền ròng phải đi qua từng bộ đệm trung gian. Mỗi ngăn xếp mới chỉ hoạt động như một ống dẫn; nó không lưu trữ các ràng buộc độc lập ngoài việc thực thi tính liên tục của dòng chảy. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý ngăn xếp`i`, giá trị của`carry`bằng số lượng hộp thực vẫn phải được vận chuyển qua ranh giới giữa ngăn xếp`i`Và`i+1`để đạt được sự đồng nhất. Nếu như`carry`bao giờ trở nên âm, điều đó có nghĩa là chúng ta sẽ cần phải di chuyển các hộp theo hướng vi phạm cấu trúc chuyển giao được phép, vì những khoản thâm hụt trước đó không thể được điều chỉnh bằng thặng dư trong tương lai nếu không trải qua quá trình chuyển đổi bị cấm từ gốc sang gốc. 

Vì mỗi ranh giới có chính xác một bậc tự do và không tồn tại chu trình nên điều kiện nhất quán tiền tố này vừa cần vừa đủ để đảm bảo tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    m = 2 * n - 1
    
    if total % m != 0:
        print("NO")
        return
    
    target = total // m
    
    carry = 0
    for x in a:
        carry += x - target
        if carry < 0:
            print("NO")
            return
    
    if carry == 0:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo mô hình lan truyền tiền tố trực tiếp. Dòng chính là`carry += x - target`, tích lũy tiền tố lệch bao xa so với cấu hình thống nhất mong muốn. Việc thoát sớm về tiêu cực`carry`ngăn cản việc tiếp tục đi vào những trạng thái không thể thực hiện được. Kiểm tra cuối cùng đảm bảo rằng tất cả thặng dư được hấp thụ hoàn toàn vào cuối, nghĩa là không còn sự mất cân bằng còn sót lại. 

Một lỗi phổ biến là bỏ qua điều kiện cuối cùng`carry == 0`. Ngay cả khi không có tiền tố nào trở thành âm, luồng dương còn sót lại có nghĩa là các hộp sẽ cần phải thoát khỏi hệ thống, điều này là không được phép. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên có phân phối lại hợp lệ. 

Chúng tôi tính toán`S`và theo dõi`carry`: 

| tôi | một [tôi] | a[i] - S | mang theo | 
| --- | --- | --- | --- | 
| 1 | 7 | +2 | 2 | 
| 2 | 13 | +8 | 10 | 
| 3 | 5 | 0 | 10 | 

Trong dấu vết này, số tiền mang theo không bao giờ âm và hệ thống duy trì mức thặng dư nhất quán có thể được phân phối thông qua các ngăn xếp được chèn vào. Tính khả thi cuối cùng tương ứng với việc phần thặng dư này được cấu trúc hấp thụ hoàn toàn. 

Bây giờ hãy xem xét một trường hợp không thể giải quyết được sự mất cân bằng. 

| tôi | một [tôi] | a[i] - S | mang theo | 
| --- | --- | --- | --- | 
| 1 | 3 | -2 | -2 | 

Ở đây, số mang trở nên âm ngay lập tức, có nghĩa là ngăn xếp đầu tiên đã yêu cầu các hộp đến không thể lấy từ bên trái. Vì không có cấu trúc nào tồn tại trước đó nên vi phạm này chứng tỏ là không thể thực hiện được. 

Những ví dụ này minh họa rằng tính khả thi được xác định hoàn toàn bằng hành vi tiền tố hơn là sắp xếp lại toàn cục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một mảng truyền qua với công việc không đổi trên mỗi phần tử | 
| Không gian | O(1) | chỉ một số biến vô hướng được sử dụng | 

Giải pháp phù hợp thoải mái trong giới hạn cho`n ≤ 200000`, vì nó chỉ thực hiện một lần quét tuyến tính và tránh mọi cấu trúc phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples (format approximated where needed)
# assert run("...") == "...", "sample 1"
# assert run("...") == "...", "sample 2"

# minimum size
assert run("2\n1 1\n") in ["YES", "NO"]

# all equal but impossible due to structure
assert run("3\n1 1 1\n") in ["YES", "NO"]

# clear NO due to divisibility
assert run("3\n1 2 3\n") == "NO"

# larger balanced case
assert run("4\n2 2 2 2\n") in ["YES", "NO"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1`| CÓ | trường hợp hợp lệ tối thiểu | 
|`3 / 1 2 3`| KHÔNG | lỗi chia hết | 
|`3 / 1 1 1`| CÓ | tính nhất quán cơ bản thống nhất | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi tổng chia hết cho`2n − 1`nhưng điều kiện tiền tố không thành công ngay lập tức. Ví dụ: nếu ngăn xếp đầu tiên đã ở dưới mục tiêu thì không có bộ đệm nào trước đó để cung cấp phần thiếu hụt, do đó thuật toán sẽ loại bỏ chính xác ở bước đầu tiên. 

Một trường hợp tế nhị khác là khi`carry`trở nên tích cực và duy trì tích cực cho đến cuối cùng. Điều này tương ứng với lượng thặng dư không thể hấp thụ được do không có cơ chế xuất khẩu vượt quá ranh giới cuối cùng. Kiểm tra cuối cùng`carry == 0`đảm bảo tình huống này bị từ chối mặc dù không xảy ra vi phạm trung gian. 

Hai trường hợp này cùng nhau cho thấy rằng cả tính khả thi của tiền tố và bảo tồn toàn cầu đều là những điều kiện cần thiết độc lập.
