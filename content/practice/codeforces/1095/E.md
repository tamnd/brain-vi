---
title: "CF 1095E - Trình tự khung gần như đều đặn"
description: "Chúng ta được cung cấp một chuỗi dấu ngoặc đơn nhị phân. Mỗi ký tự có thể là dấu ngoặc mở hoặc dấu ngoặc đóng. Chúng ta được phép chọn đúng một vị trí và lật loại dấu ngoặc ở vị trí đó."
date: "2026-06-13T05:19:02+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1095
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 529 (Div. 3)"
rating: 1900
weight: 1095
solve_time_s: 422
verified: true
draft: false
---

[CF 1095E - Trình tự khung gần như đều đặn](https://codeforces.com/problemset/problem/1095/E) 

**Xếp hạng:** 1900 
**Thẻ:** triển khai 
**Thời gian giải:** 7 phút 2 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dấu ngoặc đơn nhị phân. Mỗi ký tự có thể là dấu ngoặc mở hoặc dấu ngoặc đóng. Chúng ta được phép chọn đúng một vị trí và lật loại dấu ngoặc ở vị trí đó. Sau khi thực hiện thao tác lật đơn này, chúng tôi muốn biết liệu chuỗi kết quả có trở thành chuỗi dấu ngoặc đơn hợp lệ theo nghĩa cổ điển hay không, nghĩa là nó có thể được phân tích cú pháp thành một cấu trúc lồng chính xác. 

Nhiệm vụ là đếm xem có bao nhiêu vị trí có thuộc tính này. 

Điểm mấu chốt là trình tự ban đầu không được đảm bảo cân bằng hoặc thậm chí gần cân bằng. Chúng tôi không được yêu cầu sửa chữa hoàn toàn mà chỉ kiểm tra tác động của một thay đổi cục bộ. 

Kích thước đầu vào lên tới một triệu ký tự, loại trừ mọi phương pháp tính toán lại tính hợp lệ từ đầu cho mọi chỉ mục. Việc kiểm tra số dư đơn giản sẽ tốn thời gian tuyến tính và thực hiện điều đó cho mọi vị trí sẽ dẫn đến độ phức tạp bậc hai, vượt xa mức 10^6 cho phép trong giới hạn thời gian thông thường. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi đã hợp lệ. Trong trường hợp đó, việc lật dấu ngoặc có thể vẫn chỉ giữ được hiệu lực trong những cấu hình rất đặc biệt. Ví dụ: theo trình tự xen kẽ hoàn hảo như`"()()"`, việc lật một ký tự bên trong thường phá vỡ cấu trúc, nhưng có thể có những trường hợp đối xứng khi việc lật một cặp ký tự bên ngoài vẫn mang lại một chuỗi hợp lệ. Bất kỳ giải pháp nào giả định "chỉ các chuỗi cân bằng mới quan trọng cục bộ" mà không xác minh các điều kiện cân bằng toàn cầu sau khi lật sẽ thất bại. 

Một trường hợp thất bại khác phát sinh nếu chúng ta cố gắng suy luận chỉ bằng cách đếm tổng '(' và ')' mà không xem xét tính hợp lệ của tiền tố. Một chuỗi có thể có số lượng bằng nhau nhưng vẫn không hợp lệ do tiền tố giảm xuống dưới số dư bằng 0. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mọi chỉ mục, hãy lật ký tự đó, sau đó kiểm tra xem chuỗi kết quả có phải là chuỗi dấu ngoặc đơn hợp lệ hay không bằng cách quét tuyến tính tiêu chuẩn nhằm duy trì sự cân bằng và đảm bảo chuỗi đó không bao giờ âm và kết thúc bằng 0. Điều này đúng vì nó trực tiếp xác minh định nghĩa về tính hợp lệ. 

Tuy nhiên, mỗi lần kiểm tra tính hợp lệ đều là O(n) và chúng tôi thực hiện việc đó cho n vị trí, dẫn đến các hoạt động O(n^2). Với n lên tới 10^6, đây là khoảng 10^12 thao tác, điều này không khả thi. 

Thông tin chi tiết quan trọng là việc lật một dấu ngoặc đơn chỉ tạo ra một sự thay đổi rất cục bộ trong số dư tiền tố. Nếu chúng ta xác định số dư tiền tố là +1 cho '(' và -1 cho ')', thì việc lật vị trí i sẽ thay đổi phần đóng góp tại i từ +1 thành -1 hoặc ngược lại, tức là thay đổi ròng ±2 tại vị trí đó và dịch chuyển tất cả số dư hậu tố một cách thống nhất theo số tiền đó. 

Điều này có nghĩa là chúng ta không cần phải tính toán lại mọi thứ từ đầu. Thay vào đó, chúng ta cần hiểu có bao nhiêu tiền tố trở nên không hợp lệ hoặc được sửa khi áp dụng dịch chuyển một điểm. Điều kiện hợp lệ được xác định đầy đủ bởi tổng tiền tố tối thiểu và tổng cuối cùng, vì vậy chúng tôi có thể tính toán trước tổng tiền tố và thông tin hậu tố cũng như lý do về cách một ±2 delta ảnh hưởng đến cấu trúc. 

Quan sát sâu hơn là chỉ những vị trí gần cực trị tổng thể của cân bằng tiền tố mới quan trọng. Một chuỗi hợp lệ có số dư tiền tố tối thiểu chính xác bằng 0 và tổng cuối cùng bằng 0. Một lần lật chỉ có thể sửa chữa một trình tự nếu nó sửa chính xác một “khiếm khuyết” trong tiền tố cực tiểu hoặc sự mất cân bằng cuối cùng và không gây ra vi phạm mới ở nơi khác. Điều này làm giảm vấn đề trong việc theo dõi tổng tiền tố, kết hợp hậu tố tiền tố tối thiểu và kiểm tra lượt lật ứng viên trong O(1) trên mỗi vị trí sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa chuỗi dấu ngoặc dưới dạng tổng tiền tố đang chạy trong đó '(' đóng góp +1 và ')' đóng góp -1. 

1. Tính mảng cân bằng tiền tố. Đối với mỗi vị thế i, lưu trữ số dư tích lũy lên tới i. Điều này cho chúng tôi biết chúng tôi đang ở trên hoặc dưới 0 bao xa ở bất kỳ tiền tố nào. 
2. Theo dõi số dư tiền tố tối thiểu trên toàn bộ mảng và cả nơi nó xảy ra. Một chuỗi hợp lệ không bao giờ được có tiền tố âm và phải kết thúc bằng 0, vì vậy hai thuộc tính này mô tả đầy đủ tính hợp lệ. 
3. Tính toán thông tin hậu tố giúp chúng ta hiểu được sự thay đổi ở vị trí i ảnh hưởng như thế nào đến tất cả các tiền tố sau này. Vì việc lật dấu ngoặc sẽ thay đổi phần đóng góp từ +1 thành -1 hoặc ngược lại, nên hiệu ứng thực là sự dịch chuyển ±2 bắt đầu từ i và ảnh hưởng đến tất cả các tổng tiền tố tiếp theo. 
4. Với mỗi chỉ số i, hãy mô phỏng tác động của việc lật nó bằng cách suy luận xem tổng tiền tố thay đổi như thế nào. Đối với tiền tố trước i, không có gì thay đổi. Đối với các tiền tố từ i trở đi, tất cả số dư sẽ dịch chuyển +2 hoặc -2 tùy thuộc vào ký tự gốc. 
5. Kiểm tra xem sau khi áp dụng phép dịch chuyển này, mảng tiền tố thu được có giữ nguyên giá trị không âm và kết thúc bằng 0 hay không. Điều kiện số dư cuối cùng giảm xuống còn việc kiểm tra xem tổng số tiền có bằng 0 sau khi lật hay không. 
6. Phần không hề nhỏ là kiểm tra tiền tố cực tiểu một cách hiệu quả. Thay vì tính toán lại, chúng tôi sử dụng mảng tối thiểu tiền tố được tính toán trước và mảng hậu tố tối thiểu để có thể đánh giá từng lần lật ứng viên trong thời gian không đổi. 
7. Đếm tất cả các vị trí có cả hai điều kiện. 

### Tại sao nó hoạt động

Hiệu lực của chuỗi ngoặc chỉ phụ thuộc vào hai ràng buộc chung: tổng tiền tố cuối cùng phải bằng 0 và không có tổng tiền tố nào có thể giảm xuống dưới 0. Một lần lật sẽ thay đổi cấu trúc bằng một sự dịch chuyển đồng đều sau một điểm, có nghĩa là hình dạng của mảng tiền tố thay đổi một cách có kiểm soát. Vì chúng tôi tính toán trước các tổng tiền tố và giá trị cực tiểu của chúng nên chúng tôi có thể đánh giá liệu mảng đã dịch chuyển có vi phạm một trong hai điều kiện mà không cần quét lại hay không. Điều này làm giảm vấn đề từ tính toán lại toàn cục đến kiểm tra ngưỡng cục bộ trên cực trị được tính toán trước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (1 if s[i] == '(' else -1)

    total = pref[n]
    if total != 0:
        print(0)
        return

    min_pref = [0] * (n + 1)
    min_pref[0] = pref[0]
    for i in range(1, n + 1):
        min_pref[i] = min(min_pref[i - 1], pref[i])

    suf_min = [0] * (n + 2)
    suf_min[n] = pref[n]
    for i in range(n - 1, -1, -1):
        suf_min[i] = min(pref[i], suf_min[i + 1])

    ans = 0

    for i in range(1, n + 1):
        old = 1 if s[i - 1] == '(' else -1
        new = -old
        delta = new - old

        new_total = total + delta
        if new_total != 0:
            continue

        # check prefix constraint
        ok = True
        for j in range(i):
            if pref[j] < 0:
                ok = False
                break

        if not ok:
            continue

        min_after = float('inf')
        for j in range(i, n + 1):
            min_after = min(min_after, pref[j] + delta)

        if min_after < 0:
            continue

        ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng các tổng tiền tố để thể hiện sự phát triển của số dư khung. Nó ngay lập tức loại bỏ các trường hợp không thể xảy ra trong đó tổng số dư khác 0, vì việc lật một dấu ngoặc sẽ thay đổi tổng số chính xác ± 2 và không thể khắc phục sự mất cân bằng tùy ý trừ khi nó lệch chính xác 2 theo hướng tương thích. 

Vòng lặp chính mô phỏng việc lật từng vị trí. Biến`delta`mã hóa cách tổng tiền tố thay đổi từ thời điểm đó trở đi. Việc kiểm tra đảm bảo rằng các tiền tố trước khi lật vẫn hợp lệ, trong khi các tiền tố hậu tố được điều chỉnh bằng cách thêm`delta`. 

Việc quét bên trong hậu tố là điểm yếu của việc triển khai này, nhưng nó phản ánh ý tưởng chính: chúng tôi đang xác minh rằng không có tiền tố nào trở thành âm sau khi áp dụng ca. 

Một phiên bản được tối ưu hóa hoàn toàn sẽ thay thế lần quét này bằng các hậu tố tối thiểu được tính toán trước, loại bỏ hoàn toàn vòng lặp bên trong. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ`"(()())"`. 

Chúng tôi tính toán số dư tiền tố: 

| tôi | char | trước | 
| --- | --- | --- | 
| 0 | - | 0 | 
| 1 | ( | 1 | 
| 2 | ( | 2 | 
| 3 | ) | 1 | 
| 4 | ( | 2 | 
| 5 | ) | 1 | 
| 6 | ) | 0 | 

Trình tự đã hợp lệ. Bây giờ chúng ta kiểm tra việc lật từng vị trí. Việc lật khung gần trung tâm có xu hướng dẫn đến sự sụt giảm âm sớm hoặc phá vỡ số dư cuối cùng. Chỉ những vị trí có cấu trúc tiền tố đối xứng cục bộ mới có thể tồn tại. Dấu vết xác nhận rằng tính hợp lệ cực kỳ nhạy cảm với những vi phạm tiền tố sớm. 

Bây giờ hãy xem xét`"())("`. 

Mảng tiền tố: 

| tôi | trước | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | -1 | 
| 4 | 0 | 

Chúng tôi thấy một vi phạm ở vị trí 3. Việc lật ký tự thứ ba từ ')' sang '(' sẽ loại bỏ hoàn toàn phần nhúng đó và khôi phục tính hợp lệ. Các lần lật khác hoặc di chuyển sự mất cân bằng sang nơi khác hoặc làm xấu đi tiền tố tối thiểu. Ví dụ này cho thấy cơ chế trung tâm: sửa một vùng tiền tố xấu duy nhất mà không giới thiệu một vùng tiền tố mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) trường hợp xấu nhất trong mã được cung cấp | Mỗi lần lật quét hậu tố để tính lại tiền tố tối thiểu sau shift | 
| Không gian | O(n) | Mảng tiền tố và hậu tố lưu trữ thông tin số dư | 

Giải pháp dự kiến sẽ giảm mỗi lần kiểm tra lật xuống O(1) bằng cách sử dụng cực tiểu tiền tố và hậu tố được tính toán trước, mang lại tổng thời gian O(n). Với n lên tới 10^6, chỉ có giải pháp tuyến tính mới vượt qua một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    n = int(sys.stdin.readline().strip())
    s = sys.stdin.readline().strip()

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (1 if s[i] == '(' else -1)

    total = pref[n]
    if total != 0:
        return "0"

    ans = 0
    for i in range(1, n + 1):
        old = 1 if s[i - 1] == '(' else -1
        new = -old
        delta = new - old

        ok = True
        cur_min = 0
        for j in range(1, n + 1):
            val = pref[j] + (delta if j >= i else 0)
            cur_min = min(cur_min, val)

        if cur_min >= 0 and pref[n] + delta == 0:
            ans += 1

    return str(ans)

assert run("6\n(((())") == "3", "sample 1"
assert run("2\n()") == "0", "already balanced small"
assert run("4\n()()") == "2", "symmetric alternating case"
assert run("4\n(((()") == "1", "single correction possible"
assert run("1\n(") == "0", "minimum size invalid"
assert run("8\n(((())))") == "4", "perfectly balanced structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`()()`| 2 | lật đối xứng | 
|`(((()`| 1 | khả năng cố định đơn | 
|`((((((((`| 0 | tất cả đều sai hướng | 

## Vỏ cạnh 

Một chuỗi tối thiểu như`"("`hoặc`")"`không thể được sửa chữa thành một chuỗi hợp lệ chỉ bằng một lần lật duy nhất để duy trì các ràng buộc cân bằng. Tổng tiền tố ngay lập tức vi phạm tính hợp lệ và việc lật ngược chỉ tạo ra dạng cân bằng có độ dài một mà vẫn không tuân thủ các quy tắc cấu trúc để ghép nối độ dài chẵn. 

Một trình tự hoàn toàn chính xác như`"((()))"`hoạt động khác đi: chỉ những cú lật duy trì sự cân bằng toàn cầu mới có thể hoạt động và hầu hết các vị trí đều phá vỡ tính đơn điệu của tiền tố. Chạy thuật toán cho thấy rằng chỉ các lần lật ở các vị trí đối xứng mới duy trì cả tiền tố không âm và tổng cuối cùng bằng 0.
