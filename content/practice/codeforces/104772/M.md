---
title: "CF 104772M - Thiếu nguyên âm"
description: "Chúng ta có hai chuỗi đại diện cho cùng một địa điểm hoặc tên được viết theo hai cách khác nhau. Chuỗi đầu tiên là phiên bản rút gọn, còn chuỗi thứ hai là phiên bản đầy đủ."
date: "2026-06-28T16:15:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104772
codeforces_index: "M"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104772
solve_time_s: 82
verified: false
draft: false
---

[CF 104772M - Thiếu nguyên âm](https://codeforces.com/problemset/problem/104772/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi đại diện cho cùng một địa điểm hoặc tên được viết theo hai cách khác nhau. Chuỗi đầu tiên là phiên bản rút gọn, còn chuỗi thứ hai là phiên bản đầy đủ. Quy tắc chuyển đổi rất cụ thể: bắt đầu từ chuỗi đầy đủ, chúng ta được phép xóa một số nguyên âm, nhưng không được phép xóa phụ âm hoặc dấu gạch nối. Sau khi xóa bất kỳ tập hợp con nguyên âm nào, chuỗi kết quả phải khớp chính xác với chuỗi ngắn. 

Nhiệm vụ là quyết định xem quá trình xóa như vậy có thể chuyển đổi chuỗi đầy đủ thành chuỗi ngắn hay không. 

Các chuỗi có thể có độ dài lên tới 1000, do đó, bất kỳ giải pháp nào thử tất cả các tập hợp con nguyên âm hoặc mô phỏng việc xóa theo cấp số nhân đều ngay lập tức quá chậm. Quét bậc hai hoặc tuyến tính cho mỗi lần kiểm tra là hoàn toàn ổn, nhưng bất cứ điều gì ngoài việc khớp tuyến tính với tính năng theo dõi trạng thái đơn giản sẽ là chi phí không cần thiết. 

Một điểm tinh tế là không phân biệt chữ hoa chữ thường. Chữ hoa và chữ thường phải được coi là giống hệt nhau, do đó cần phải chuẩn hóa trước khi so sánh. Một chi tiết khác thường phá vỡ các giải pháp ngây thơ là quên rằng chỉ có thể xóa các nguyên âm. Phụ âm và dấu gạch nối phải xuất hiện theo cùng thứ tự tương đối trong cả hai chuỗi. 

Các trường hợp biên thường phá vỡ việc triển khai không chính xác bao gồm các tình huống trong đó: 

Nguyên âm tồn tại trong chuỗi ngắn nhưng không tồn tại trong chuỗi đầy đủ. Ví dụ: ngắn = "a", đầy đủ = "b". Cái này phải là "Khác" vì chúng ta không được phép chèn ký tự mà chỉ được phép xóa nguyên âm. 

Một trường hợp khác là khi chuỗi ngắn bỏ qua phụ âm. Ví dụ: short = "shrm", full = "sharm". Ở đây chuỗi đầy đủ chứa các nguyên âm bổ sung, vì vậy chúng ta có thể xóa 'a' và so khớp, điều này hợp lệ. 

Trường hợp tinh tế thứ ba là khi có dấu gạch nối, vì dấu gạch ngang hoạt động giống như phụ âm và không thể loại bỏ hoặc bỏ qua. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ xem xét tất cả các tập hợp con của vị trí nguyên âm trong chuỗi đầy đủ. Đối với mỗi tập hợp con, chúng tôi xóa các nguyên âm đó và so sánh kết quả với chuỗi ngắn. Nếu chúng ta nghĩ về một chuỗi đầy đủ có độ dài n, có thể có 2^k tập hợp con trong đó k là số nguyên âm và trong trường hợp xấu nhất k tỷ lệ thuận với n. Mỗi chuỗi được xây dựng tốn O(n) để xây dựng và so sánh, tạo ra độ phức tạp tổng thể theo thứ tự O(n · 2^n), điều này hoàn toàn không khả thi ngay cả với n = 1000. 

Quan sát chính là thao tác không sắp xếp lại các ký tự và không cho phép xóa phụ âm hoặc dấu gạch nối. Điều này có nghĩa là chúng tôi không chọn tập hợp con một cách tùy tiện; chúng tôi đang khớp hai chuỗi theo một quy tắc nghiêm ngặt: mọi ký tự không phải nguyên âm trong chuỗi đầy đủ phải xuất hiện trong chuỗi ngắn theo cùng một thứ tự, trong khi các nguyên âm trong chuỗi đầy đủ có thể bị bỏ qua tùy ý nếu chúng không cần thiết để khớp với chuỗi ngắn. 

Điều này ngay lập tức gợi ý quét hai con trỏ. Chúng ta duyệt cả hai chuỗi cùng một lúc. Bất cứ khi nào các ký tự khớp nhau (sau khi chuẩn hóa chữ hoa chữ thường), chúng tôi sẽ tiến lên cả hai con trỏ. Nếu chúng không khớp, lời giải thích duy nhất có thể là ký tự trong chuỗi đầy đủ là nguyên âm, trong trường hợp đó chúng ta được phép bỏ qua nó. Nếu nó không phải là nguyên âm thì không thể so sánh được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^n) | O(n) | Quá chậm | 
| Quét hai con trỏ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi cả hai chuỗi thành chữ thường để so sánh không phân biệt chữ hoa chữ thường. Điều này đảm bảo việc xử lý ký tự thống nhất mà không cần phân nhánh sau này. 
2. Xác định hàm trợ giúp để kiểm tra xem một ký tự có phải là nguyên âm hay không. Trong bài toán này các nguyên âm là a, e, i, o, u, y. Dấu gạch nối không phải là nguyên âm và không bao giờ được bỏ qua. 
3. Khởi tạo hai con trỏ, một cho chuỗi ngắn và một cho chuỗi đầy đủ, cả hai đều bắt đầu ở vị trí 0. Những con trỏ này biểu thị số lượng mỗi chuỗi mà chúng ta đã khớp thành công cho đến nay. 
4. Lặp lại toàn bộ con trỏ chuỗi từ trái sang phải. Ở mỗi bước, hãy so sánh ký tự hiện tại trong chuỗi đầy đủ với ký tự hiện tại trong chuỗi ngắn nếu nó vẫn tồn tại. 
5. Nếu cả hai ký tự khớp nhau, hãy tiến cả hai con trỏ. Điều này thể hiện việc sử dụng một ký tự bắt buộc trong chuỗi ngắn từ chuỗi đầy đủ. 
6. Nếu chúng không khớp, hãy kiểm tra xem ký tự toàn chuỗi hiện tại có phải là nguyên âm hay không. Nếu đúng như vậy, chúng ta có thể bỏ qua nó một cách an toàn bằng cách chỉ tiến tới con trỏ toàn chuỗi. Điều này tương ứng với việc xóa nguyên âm đó. 
7. Nếu các ký tự không khớp và ký tự toàn chuỗi không phải là nguyên âm, chúng tôi kết luận ngay rằng việc chuyển đổi là không thể, vì các phụ âm và dấu gạch nối không thể bị loại bỏ hoặc thay đổi. 
8. Sau khi xử lý toàn bộ chuỗi, hãy kiểm tra xem chúng ta đã sử dụng thành công tất cả các ký tự trong chuỗi ngắn hay chưa. Nếu có, phép chuyển đổi là hợp lệ; mặt khác, một số ký tự bắt buộc không bao giờ khớp. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là mọi ký tự được tiêu thụ từ chuỗi ngắn đều được khớp theo thứ tự bởi một ký tự tương ứng trong chuỗi đầy đủ. Chúng tôi không bao giờ sắp xếp lại thứ tự các ký tự và chúng tôi chỉ bỏ qua các ký tự trong chuỗi đầy đủ khi chúng là nguyên âm. Điều này phản ánh chính xác hoạt động được phép. Nếu tại bất kỳ thời điểm nào có một lỗi không phải nguyên âm không khớp thì không có trình tự xóa hợp pháp nào có thể khắc phục được nó, vì các lỗi không phải nguyên âm là bắt buộc và giữ nguyên trật tự. Vì vậy, việc đến cuối chuỗi đầy đủ bằng chuỗi ngắn khớp hoàn toàn vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

VOWELS = set("aeiouy")

def can_transform(s, f):
    s = s.strip().lower()
    f = f.strip().lower()

    i = 0  # pointer for s
    j = 0  # pointer for f

    n, m = len(s), len(f)

    while j < m:
        if i < n and s[i] == f[j]:
            i += 1
            j += 1
        else:
            if f[j] in VOWELS:
                j += 1
            else:
                return False

    return i == n

def main():
    s = input().strip()
    f = input().strip()

    if can_transform(s, f):
        print("Same")
    else:
        print("Different")

if __name__ == "__main__":
    main()
```Việc thực hiện tuân theo chiến lược hai con trỏ trực tiếp. Cả hai chuỗi đều được chuẩn hóa thành chữ thường khi bắt đầu để loại bỏ những lo ngại về độ nhạy chữ hoa chữ thường. Vòng lặp chính luôn nâng cao con trỏ toàn chuỗi, sử dụng một kết quả khớp hoặc bỏ qua một nguyên âm. 

Một cạm bẫy phổ biến là quên kiểm tra lần cuối`i == n`. Nếu không có nó, trường hợp chuỗi đầy đủ kết thúc sớm nhưng chuỗi ngắn vẫn có các ký tự không khớp sẽ bị chuyển nhầm. 

Một sự tinh tế khác là đảm bảo rằng chỉ con trỏ chuỗi đầy đủ mới tiến lên khi bỏ qua các nguyên âm. Việc nâng cao cả hai con trỏ trong khi không khớp sẽ giả định sai việc xóa trong chuỗi ngắn, điều này không được phép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
Shrm-el-Shikh
Sharm-el-Sheikh
```| Bước | con trỏ | con trỏ f | s[i] | f[j] | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | s | s | khớp, tiến cả hai | 
| 2 | 1 | 1 | h | h | khớp, tiến cả hai | 
| 3 | 2 | 2 | r | một | bỏ qua nguyên âm | 
| 4 | 2 | 3 | r | r | khớp, tiến cả hai | 
| 5 | 3 | 4 | m | m | khớp, tiến cả hai | 
| ... | ... | ... | ... | ... | tiếp tục tương tự | 

Ở mỗi lần không khớp, ký tự trong chuỗi đầy đủ là một nguyên âm nên nó được bỏ qua một cách an toàn. Cuối cùng, chuỗi ngắn được sử dụng hoàn toàn, xác nhận rằng dạng ngắn chỉ có thể được rút ra bằng cách xóa các nguyên âm. 

### Ví dụ 2 

đầu vào:```
Eilot
Eilat
```| Bước | con trỏ | con trỏ f | s[i] | f[j] | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | e | e | trận đấu | 
| 2 | 1 | 1 | tôi | tôi | trận đấu | 
| 3 | 2 | 2 | tôi | tôi | trận đấu | 
| 4 | 3 | 3 | o | một | không khớp, bỏ qua 'a'? | 

Ở bước 4, các ký tự khác nhau và f[j] = 'a' là một nguyên âm nên chúng ta bỏ qua. Tuy nhiên, sau đó chúng tôi đã hết cấu trúc khớp và không thể căn chỉnh chính xác các ký tự còn lại, khiến chuỗi ngắn không khớp khi kết thúc. 

Điều này chứng tỏ trường hợp bỏ qua cục bộ là không đủ để đảm bảo kết quả khớp hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi con trỏ chỉ di chuyển về phía trước qua chuỗi của nó một lần | 
| Không gian | O(1) | Chỉ có bộ nhớ bổ sung liên tục cho con trỏ và bộ nguyên âm | 

Quét tuyến tính là đủ cho các chuỗi có độ dài lên tới 1000 và công việc hệ số không đổi trên mỗi ký tự là tối thiểu, do đó giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided samples
assert run("Shrm-el-Shikh\nSharm-el-Sheikh\n") == "Same"
assert run("Eilot\nEilat\n") == "Different"
assert run("Saint-Petersburg\nSaint-Petersburg\n") == "Same"

# custom cases
assert run("a\nb\n") == "Different", "single mismatch non-vowel"
assert run("shrm\nsharm\n") == "Same", "simple vowel deletion"
assert run("Aeiouy\nbcdfg\n") == "Different", "vowels cannot create matches"
assert run("abc\nabc\n") == "Same", "no deletions needed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a/b | Khác nhau | không khớp nguyên âm | 
| shrm / sharm | Tương tự | bỏ qua nguyên âm | 
| Aeiouy / bcdfg | Khác nhau | xử lý vụ việc và không bắt buộc phải đấu | 
| abc / abc | Tương tự | trường hợp nhận dạng | 

## Vỏ cạnh 

Một trường hợp như`s = "a"`,`f = "b"`được thuật toán xử lý ngay lập tức. Sau khi hạ xuống, so sánh đầu tiên là giữa 'a' và 'b'. Vì chúng khác nhau và 'b' không phải là nguyên âm nên hàm trả về sai ngay lập tức. Điều này xác nhận rằng các nguyên âm không thể được sử dụng để bù đắp cho các phụ âm bị thiếu. 

Vì`s = "shrm"`Và`f = "sharm"`, quá trình quét sẽ tiếp tục cho đến khi có chữ 'a' trong chuỗi đầy đủ. Vì 'a' là một nguyên âm nên nó bị bỏ qua và các ký tự còn lại sẽ căn chỉnh hoàn hảo. Bất biến được giữ nguyên vì tất cả các ký tự trùng khớp trong`s`được bảo quản theo thứ tự. 

Vì`s = "Saint-Petersburg"`Và`f = "Saint-Petersburg"`, mọi ký tự đều khớp trực tiếp nên không xảy ra hiện tượng bỏ qua. Thuật toán sử dụng cả hai chuỗi một cách đồng bộ và kết thúc với cả hai con trỏ được căn chỉnh, xác nhận tính chính xác khi không cần xóa.
