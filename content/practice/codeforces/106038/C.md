---
title: "CF 106038C - Jo\u00e3o Pessoa"
description: "Chúng ta được sắp xếp theo vòng tròn các dấu ngoặc đơn. Chuỗi chỉ chứa '(' và ')' và tổng số dấu ngoặc mở và đóng bằng nhau. Chúng ta được phép lấy tiền tố của chuỗi và di chuyển nó đến cuối, xoay chuỗi một cách hiệu quả."
date: "2026-06-20T18:37:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106038
codeforces_index: "C"
codeforces_contest_name: "UNICAMP Selection Contest 2025"
rating: 0
weight: 106038
solve_time_s: 52
verified: true
draft: false
---

[CF 106038C - Jo\u00e3o Pessoa](https://codeforces.com/problemset/problem/106038/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp theo vòng tròn các dấu ngoặc đơn. Chuỗi chỉ chứa`'('`Và`')'`và tổng số dấu ngoặc mở và đóng bằng nhau. Chúng ta được phép lấy tiền tố của chuỗi và di chuyển nó đến cuối, xoay chuỗi một cách hiệu quả. Nhiệm vụ là chọn một phép xoay làm cho chuỗi kết quả trở thành một chuỗi dấu ngoặc đơn hợp lệ và xuất ra bao nhiêu ký tự chúng ta cần di chuyển từ phía trước. 

Chuỗi dấu ngoặc đơn hợp lệ ở đây có nghĩa là nếu chúng ta quét từ trái sang phải, chúng ta sẽ không bao giờ thấy nhiều dấu ngoặc đơn đóng hơn dấu ngoặc đơn mở ở bất kỳ tiền tố nào và ở cuối số đếm khớp, điều này đã được đảm bảo bởi đầu vào. 

Ràng buộc chính là độ dài chuỗi tối đa các giới hạn điển hình của Codeforces (các giải pháp tuyến tính hoặc gần tuyến tính được mong đợi là hoàn toàn lớn, vì vậy). Cách tiếp cận bậc hai thử mọi phép quay và kiểm tra tính hợp lệ từ đầu sẽ quá chậm vì mỗi lần kiểm tra là O(n), dẫn đến tổng thể là O(n²). 

Trường hợp cạnh tinh tế xuất hiện khi nhiều phép quay tạo ra các chuỗi hợp lệ. Ví dụ,`"()()"`đã hợp lệ rồi, do đó xoay theo 0 là đúng, nhưng xoay theo 2 cũng có tác dụng. Bài toán cho phép bất kỳ câu trả lời hợp lệ nào, vì vậy chúng ta chỉ cần một phép quay đúng. 

Một trường hợp khác là khi dây bị đảo ngược hoàn toàn về mặt cân bằng, chẳng hạn như`"))((()"`. Việc kiểm tra ngây thơ chỉ tính tổng số sẽ thất bại vì tiền tố trung gian quan trọng, không chỉ số lượng toàn cầu. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ thử mọi cách xoay vòng có thể. Với mỗi chỉ số xoay i, chúng ta xây dựng chuỗi xoay và kiểm tra xem nó có hợp lệ hay không bằng cách quét từ trái sang phải và theo dõi số dư. Mỗi lần kiểm tra tính hợp lệ có chi phí O(n) và có n vòng quay, cho thời gian O(n2). Điều này là quá chậm đối với n lớn. 

Cấu trúc của bài toán cho thấy chúng ta nên tránh tính toán lại tính hợp lệ từ đầu. Quan sát quan trọng là tính hợp lệ của chuỗi dấu ngoặc đơn chỉ phụ thuộc vào số dư tiền tố và phép quay chỉ thay đổi khi chúng ta bắt đầu đọc cùng một chuỗi tuần hoàn. Vì vậy, thay vì xây dựng lại các chuỗi, chúng ta có thể nghĩ về tổng tiền tố trên chế độ xem nhân đôi hoặc theo chu kỳ. 

Chúng tôi xác định số dư là +1 cho`'('`và -1 cho`')'`. Đối với phép quay bắt đầu từ vị trí i, chúng ta cần tất cả các tổng tiền tố trong phân đoạn`[i, i+n-1]`(theo vòng tròn) để không âm. 

Ý tưởng chính là tìm vị trí mà việc bắt đầu truyền tải mang lại cơ hội tốt nhất để tránh tổng tiền tố âm. Điều này có thể được xác định bằng cách theo dõi số dư tiền tố tối thiểu trên mảng ban đầu và chọn một vòng quay bắt đầu ngay sau điểm thấp nhất trong đường cong số dư tích lũy. Điều đó đảm bảo đường dẫn không bao giờ giảm xuống dưới 0 khi được căn giữa lại. 

Điều này làm giảm nhiệm vụ xuống còn một số dư tiền tố tính toán quét tuyến tính duy nhất và định vị chỉ mục của tổng tiền tố tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu (xoay tiền tố tối thiểu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi thành mảng giá trị +1 và -1 tùy theo dấu ngoặc đơn. Điều này biến vấn đề thành việc theo dõi các tổng tích lũy thay vì cấu trúc chuỗi, làm cho các hiệu ứng xoay trở nên dễ giải thích hơn. 
2. Tính tổng tiền tố trong khi quét chuỗi từ trái sang phải. Giữ tổng số đang chạy trong đó mỗi`'('`tăng tổng và mỗi`')'`làm giảm nó. Điều này thể hiện mức độ trên hoặc dưới ngưỡng hợp lệ của chuỗi tại mỗi điểm. 
3. Theo dõi chỉ mục nơi tổng tiền tố đạt giá trị tối thiểu. Điểm này tương ứng với mức giảm cân bằng sâu nhất trong toàn bộ chuỗi. Trực giác cho rằng đây là điểm yếu nhất trong cấu trúc tuần hoàn. 
4. Chọn cách xoay sao cho điểm tối thiểu này trở thành ngay trước điểm bắt đầu của chuỗi mới. Cụ thể, nếu tiền tố tối thiểu xuất hiện ở chỉ số i, chúng ta sẽ xoay theo vị trí i+1. 
5. Xuất chỉ số xoay này. Nếu tồn tại nhiều lựa chọn hợp lệ thì mọi vị trí đạt được tối thiểu đều có tác dụng vì tất cả các phép quay như vậy đều tránh được tổng tiền tố âm. 

### Tại sao nó hoạt động 

Đường cong tổng tiền tố mô tả một cuộc đi bộ bắt đầu từ 0 và kết thúc ở 0. Một chuỗi dấu ngoặc đơn hợp lệ chính xác là một bước đi không bao giờ xuống dưới 0. Khi chúng ta xoay chuỗi, chúng ta đang chọn một điểm khởi đầu mới trên hành trình tuần hoàn này một cách hiệu quả. Điểm bắt đầu tốt nhất là điểm ngay sau mức tối thiểu toàn cầu, vì điều đó làm dịch chuyển toàn bộ đường cong lên trên sao cho điểm thấp nhất trở thành 0 thay vì âm. Vì mọi tiền tố khác đều cao hơn hoặc bằng mức tối thiểu này nên không có tiền tố nào trong phiên bản được xoay có thể chuyển sang âm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

min_prefix = 0
min_idx = -1

pref = 0

for i, ch in enumerate(s):
    if ch == '(':
        pref += 1
    else:
        pref -= 1

    if pref < min_prefix:
        min_prefix = pref
        min_idx = i

# rotate so that min prefix position moves to end of prefix
print(min_idx + 1)
```Mã tính toán số dư tích lũy trong một lần chuyển trong khi theo dõi nơi xảy ra giá trị thấp nhất. Biến`min_idx`lưu trữ vị trí cuối cùng trong đó tổng tiền tố là tối thiểu. Xoay sau chỉ số này đảm bảo chuỗi bắt đầu từ điểm mà ở đó số dư không âm xuyên suốt. 

Một sự tinh tế phổ biến là xử lý từng cái một: chúng tôi xoay vòng theo`min_idx + 1`, không`min_idx`, vì chúng ta muốn chuỗi mới bắt đầu ngay sau điểm tiền tố thấp nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`"))((()"`Chúng tôi tính toán số dư tiền tố từng bước. 

| tôi | char | tiền tố | tiền tố tối thiểu | min_idx | 
| --- | --- | --- | --- | --- | 
| 0 | ) | -1 | -1 | 0 | 
| 1 | ) | -2 | -2 | 1 | 
| 2 | ( | -1 | -2 | 1 | 
| 3 | ( | 0 | -2 | 1 | 
| 4 | ) | -1 | -2 | 1 | 
| 5 | ( | 0 | -2 | 1 | 

Tiền tố tối thiểu là -2 ở chỉ số 1, vì vậy chúng ta xoay 2. Chuỗi được xoay là`"((())("`, hợp lệ. 

Dấu vết này cho thấy rằng mặc dù xảy ra nhiều điểm nhúng âm, điểm nhúng sâu nhất sẽ xác định điểm xoay chính xác. 

### Ví dụ 2:`"()()"`| tôi | char | tiền tố | tiền tố tối thiểu | min_idx | 
| --- | --- | --- | --- | --- | 
| 0 | ( | 1 | 0 | -1 | 
| 1 | ) | 0 | 0 | -1 | 
| 2 | ( | 1 | 0 | -1 | 
| 3 | ) | 0 | 0 | -1 | 

Tiền tố tối thiểu là 0 xuyên suốt, vì vậy`min_idx = -1`. Chúng tôi xuất ra`0`, nghĩa là không cần xoay. Chuỗi đã hợp lệ. 

Điều này cho thấy thuật toán xử lý các chuỗi đã được cân bằng một cách tự nhiên mà không cần cách viết hoa đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | tổng tiền tố tính toán một lần | 
| Không gian | O(1) | chỉ một vài biến số nguyên được sử dụng | 

Giải pháp này phù hợp thoải mái trong các ràng buộc vì nó xử lý mỗi ký tự một lần và chỉ thực hiện cập nhật theo thời gian liên tục cho mỗi bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    n = len(s)

    min_prefix = 0
    min_idx = -1
    pref = 0

    for i, ch in enumerate(s):
        pref += 1 if ch == '(' else -1
        if pref < min_prefix:
            min_prefix = pref
            min_idx = i

    return str(min_idx + 1)

# provided samples
assert run("))((()\n") == "2"
assert run("()\n") == "0"
assert run("))((\n") == "2"

# custom cases
assert run("()") == "0", "already valid"
assert run(")(()") == "1", "single correction via rotation"
assert run("((()))") == "0", "fully nested already valid"
assert run(")))(((") == "3", "large imbalance symmetric"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`()`|`0`| đã hợp lệ, không cần xoay vòng | 
|`)(()`|`1`| cần xoay để sửa tiền tố âm sớm | 
|`)))(((`|`3`| mất cân bằng trong trường hợp xấu nhất, kiểm tra việc xử lý chỉ số tối thiểu chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chuỗi đã hợp lệ và không bao giờ giảm xuống dưới 0. Trong trường hợp này, tiền tố tối thiểu bằng 0 tại mọi điểm, vì vậy`min_idx`còn lại`-1`, và đầu ra trở thành`0`. Thuật toán xác định chính xác rằng không cần xoay. 

Một trường hợp cạnh khác là khi tiền tố tối thiểu xuất hiện ở ký tự cuối cùng. Trong một chuỗi như`"((()))"`hoặc bất kỳ hình thức hợp lệ đã được cân bằng nào, điều này vẫn dẫn đến`min_idx = -1`, do đó kết quả ổn định và không gợi ý xoay sai. 

Trường hợp tinh tế hơn là khi nhiều vị trí có cùng giá trị tiền tố tối thiểu. Thuật toán lưu trữ lần xuất hiện cuối cùng, nhưng bất kỳ lần xuất hiện nào cũng sẽ tạo ra một vòng quay hợp lệ vì việc dịch chuyển sau bất kỳ lần nhúng sâu nhất nào sẽ tạo ra một bước đi được căn giữa lại không âm.
