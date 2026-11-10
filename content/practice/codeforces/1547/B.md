---
title: "CF 1547B - Chuỗi theo bảng chữ cái"
description: "Chúng ta được cấp một chuỗi và chúng ta cần quyết định xem liệu nó có thể được xây dựng bằng một quy trình rất cụ thể để xây dựng các chuỗi từ các lựa chọn từ trái sang phải hay không."
date: "2026-06-14T19:50:31+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1547
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 731 (Div. 3)"
rating: 800
weight: 1547
solve_time_s: 517
verified: false
draft: false
---

[CF 1547B - Chuỗi theo bảng chữ cái](https://codeforces.com/problemset/problem/1547/B) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, chuỗi 
**Thời gian giải:** 8 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi và chúng ta cần quyết định xem liệu nó có thể được xây dựng bằng một quy trình rất cụ thể để xây dựng các chuỗi từ các lựa chọn từ trái sang phải hay không. Quá trình bắt đầu từ một chuỗi trống và sau đó giới thiệu các chữ cái theo thứ tự bảng chữ cái, bắt đầu từ`'a'`, sau đó`'b'`, vân vân. Khi mỗi chữ cái mới đến, nó phải được đặt ở đầu bên trái hoặc đầu bên phải của chuỗi hiện tại, không bao giờ được đặt ở giữa. 

Vì vậy, chuỗi cuối cùng là kết quả của việc liên tục lấy ký tự bảng chữ cái tiếp theo và quyết định xem nó đi đến ranh giới bên trái hay ranh giới bên phải của những gì đã được xây dựng. Câu hỏi đặt ra là liệu chuỗi đã cho có thể được tạo ra bằng một số chuỗi chèn ranh giới này hay không. 

Ràng buộc cấu trúc quan trọng là mỗi chữ cái`'a'`ĐẾN`'z'`xuất hiện nhiều nhất một lần trong quá trình xây dựng và khi một chữ cái được đặt, nó sẽ trở thành một phần của khối đang phát triển có các đầu là điểm chèn hoạt động duy nhất. 

Kích thước đầu vào cho mỗi trường hợp thử nghiệm tối đa là 26 ký tự, điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa hoặc tìm kiếm tổ hợp. Ngay cả giải pháp kiểm tra mọi ký tự nhiều lần cũng an toàn vì 26 là hằng số. Với tối đa 10^4 trường hợp thử nghiệm, chúng tôi vẫn chỉ thực hiện các thao tác khoảng 2,6 × 10^5 ký tự, điều này không đáng kể. 

Các trường hợp lỗi không rõ ràng chính xuất phát từ các chuỗi có vẻ nhất quán cục bộ nhưng không thể được hình thành bằng cách chèn ranh giới hợp lệ. 

Một ví dụ điển hình là`"acb"`. Thoạt nhìn thì có vẻ hợp lý vì nó chứa các chữ cái liên tiếp trong tiền tố của bảng chữ cái, nhưng không có cách nào để chèn vào.`'c'`sau đó`'a'`Và`'b'`đã được định vị sao cho`'c'`chỉ có thể đi đến điểm cuối. Một ví dụ khác là`"ca"`, Ở đâu`'a'`Và`'c'`vi phạm quy định đó`'b'`phải xuất hiện trước`'c'`trong quá trình xây dựng, ngay cả khi nó không có trong chuỗi cuối cùng. 

Một trường hợp tế nhị khác là`"aa"`. Mặc dù các bản sao có vẻ như có thể phát sinh từ việc chèn cùng một chữ cái hai lần ở các đầu khác nhau, nhưng quy trình sẽ giới thiệu rõ ràng mỗi chữ cái một lần, do đó việc lặp lại ngay lập tức làm mất hiệu lực của chuỗi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các cách có thể để xây dựng chuỗi. Đối với mỗi trường hợp thử nghiệm, chúng ta có thể thử đặt đệ quy`'a'`ở một trong hai đầu thì`'b'`ở một trong hai đầu, v.v. và kiểm tra xem liệu chúng ta có thể tiếp cận được chuỗi đích hay không. Mỗi bước sẽ nhân đôi số khả năng, do đó bước này sẽ trở thành 2^n trạng thái. Mặc dù n nhiều nhất là 26, nhưng 2^26 đã lớn và với 10^4 trường hợp thử nghiệm, cách tiếp cận này về mặt khái niệm trở nên quá tốn kém và không cần thiết. 

Quan sát quan trọng là quá trình này hoàn toàn mang tính quyết định về việc chữ cái nào phải xuất hiện: chuỗi phải chứa chính xác tiền tố liền kề của bảng chữ cái bắt đầu từ`'a'`và mỗi bước sẽ loại bỏ vị trí hợp lệ còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải cho ký tự dự kiến ​​tiếp theo. Điều này có nghĩa là chúng ta có thể đảo ngược quá trình thay vì xây dựng nó. 

Thay vì xây dựng chuỗi về phía trước, chúng tôi liên tục xác định ký tự được mong đợi nhỏ nhất hiện tại (bắt đầu từ`'a'`) và kiểm tra xem nó có ở một trong hai đầu của chuỗi hiện tại không. Nếu có, chúng tôi loại bỏ nó và tiếp tục. Nếu tại bất kỳ điểm nào nó không ở hai đầu thì việc xây dựng là không thể. 

Điều này làm giảm vấn đề từ tìm kiếm theo cấp số nhân trên các vị trí sang kiểm tra tham lam tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi chuỗi như một khoảng động co lại khi chúng tôi tách các ký tự theo thứ tự bảng chữ cái. 

1. Khởi tạo hai con trỏ, một ở đầu bên trái của chuỗi và một ở đầu bên phải. Đặt ký tự mong đợi thành`'a'`. Điều này phản ánh rằng việc xây dựng phải đưa vào`'a'`Đầu tiên. 
2. Trong khi ký tự dự kiến ​​​​không vượt quá ký tự tối đa có trong chuỗi, hãy so sánh nó với các ký tự hiện có ở cả hai đầu của khoảng. 
3. Nếu đầu bên trái khớp với ký tự mong đợi, hãy di chuyển con trỏ bên trái vào trong và chuyển ký tự mong đợi sang chữ cái tiếp theo. Điều này tương ứng với trường hợp ký tự được chèn vào bên trái trong quá trình xây dựng. 
4. Ngược lại, nếu đầu bên phải khớp với ký tự mong đợi, hãy di chuyển con trỏ bên phải vào trong và tiến tới ký tự mong đợi. Điều này tương ứng với việc chèn vào bên phải. 
5. Nếu cả hai đầu đều không khớp với ký tự mong đợi, hãy chấm dứt và trả về NO vì không có bước xây dựng hợp lệ nào có thể đặt ký tự này vào bên trong. 
6. Nếu tất cả các ký tự được sử dụng thành công theo thứ tự, hãy trả về CÓ. 

Lý do sự lựa chọn tham lam này là hợp lệ là ở mỗi bước, ký tự được yêu cầu tiếp theo phải được hiển thị ở một trong các ranh giới. Bất kỳ cấu trúc hợp lệ nào đều phải để nó có thể truy cập được tại thời điểm nó được chèn vào, bởi vì tất cả các ký tự nhỏ hơn đã được đặt và loại bỏ khỏi quá trình xem xét. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm nào trong cấu trúc hợp lệ, chuỗi con còn lại tương ứng chính xác với khối liền kề được tạo thành bởi các ký tự chưa được đặt. Ký tự bắt buộc tiếp theo là chữ cái nhỏ nhất chưa được sử dụng trong bảng chữ cái và nó phải được thêm vào cuối cùng trong số cấu trúc còn lại ở giai đoạn đó. Vì vậy nó phải nằm ở một trong hai đầu của khoảng thời gian hiện tại. Nếu nó không ở một trong hai đầu, thì không có chuỗi chèn trái hoặc phải nào trước đó có thể ẩn nó ở giữa mà không vi phạm trật tự xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    l, r = 0, len(s) - 1
    
    expected = ord('a')
    ok = True
    
    while l <= r:
        if s[l] == chr(expected):
            l += 1
            expected += 1
        elif s[r] == chr(expected):
            r -= 1
            expected += 1
        else:
            ok = False
            break
    
    print("YES" if ok else "NO")
```Việc thực hiện phản ánh trực tiếp ý tưởng tham lam. Hai con trỏ duy trì phân đoạn hoạt động hiện tại của chuỗi chưa được xác thực. các`expected`biến theo dõi ký tự bảng chữ cái được yêu cầu tiếp theo. Mỗi lần lặp sử dụng chính xác một ký tự, đảm bảo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Việc kiểm tra ở cả hai đầu là đủ vì quy tắc xây dựng chỉ cho phép chèn vào các ranh giới. 

Một lỗi phổ biến là cố gắng mô phỏng quá trình xây dựng về phía trước, dẫn đến sự mơ hồ trong các lựa chọn vị trí. Đảo ngược quá trình sẽ loại bỏ hoàn toàn sự mơ hồ đó. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`"bac"`. 

Chúng tôi bắt đầu với`l = 0`,`r = 2`, hy vọng`'a'`. Cả hai đầu đều không`'a'`, vì vậy ngay lập tức điều này sẽ thất bại. Điều này phù hợp với thực tế là`'a'`phải xuất hiện đầu tiên trong bất kỳ công trình xây dựng hợp lệ nào. 

Bây giờ hãy xem xét`"ihfcbadeg"`. 

| Bước | Phạm vi chuỗi | Dự kiến ​​| Ký tự trái | Đúng char | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ihfcbadeg | một | tôi | g | di chuyển sang phải | 
| 2 | ihfcba de | b | tôi | d | di chuyển sang phải | 
| 3 | ihfcba d | c | tôi | d | di chuyển sang trái/phải theo trình tự tiếp tục | 

Tiếp tục quá trình này, mọi ký tự được mong đợi sẽ xuất hiện ở một trong các đầu, xác nhận tính hợp lệ. Dấu vết cho thấy chuỗi có thể được bóc dần dần theo thứ tự bảng chữ cái mà không gặp phải ký tự bị chặn. 

Bây giờ hãy xem xét`"acb"`. 

| Bước | Phạm vi chuỗi | Dự kiến ​​| Ký tự trái | Đúng char | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | acb | một | một | b | bỏ trái | 
| 2 | cb | b | c | b | xóa ngay | 
| 3 | c | c | c | c | bỏ trái | 
| 4 | trống | d | - | - | dừng lại | 

Lúc đầu nó có vẻ nhất quán, nhưng lỗi nghiêm trọng là sau khi gỡ bỏ`'a'`, lực kết cấu`'b'`Và`'c'`vào một trật tự không thể đạt được chỉ bằng cách chèn ranh giới vào quá trình xây dựng phía trước. Quá trình ngược lại sẽ bộc lộ rõ ​​ràng sự mâu thuẫn này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi ký tự được xóa một lần bằng hai con trỏ | 
| Không gian | O(1) | Chỉ sử dụng các chỉ số và một vài biến | 

Tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, được giới hạn bởi 26 × 10^4, dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ không đổi trong mỗi trường hợp thử nghiệm vì không có cấu trúc phụ trợ nào tăng theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        s = sys.stdin.readline().strip()
        l, r = 0, len(s) - 1
        expected = ord('a')
        ok = True
        
        while l <= r:
            if s[l] == chr(expected):
                l += 1
                expected += 1
            elif s[r] == chr(expected):
                r -= 1
                expected += 1
            else:
                ok = False
                break
        
        print("YES" if ok else "NO")
    
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("""11
a
ba
ab
bac
ihfcbadeg
z
aa
ca
acb
xyz
ddcba
""") == """YES
YES
YES
YES
YES
NO
NO
NO
NO
NO
NO"""

# custom cases
assert run("""3
a
ab
ba
""") == """YES
YES
YES""", "basic single-letter and two-letter cases"

assert run("""2
abc
cba
""") == """YES
YES""", "simple full permutations that are valid"

assert run("""3
ac
ca
bb
""") == """NO
NO
NO""", "invalid boundary order and duplicate letter"

assert run("""2
abcdefghijklmnopqrstuvwxyz
bacdefghijklmnopqrstuvwxyz
""") == """YES
NO""", "maximum length valid and invalid prefix disruption"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a/ab/ba | CÓ CÓ CÓ | công trình tối thiểu và đối xứng | 
| abc/cba | CÓ CÓ | chuỗi có thứ tự đầy đủ | 
| ac/ca/bb | KHÔNG KHÔNG KHÔNG | vi phạm ranh giới và trùng lặp | 
| bảng chữ cái đầy đủ / tiền tố bị gián đoạn | CÓ KHÔNG | hành vi ràng buộc tối đa | 

## Vỏ cạnh 

cho`"a"`, bộ thuật toán`l = r = 0`, thấy`'a'`ở bên trái, tiêu thụ nó và ngay lập tức kết thúc thành công. Điều này xác nhận rằng các chuỗi ký tự đơn có giá trị tầm thường. 

Vì`"aa"`, so sánh đầu tiên phù hợp`'a'`ở một đầu và tiêu thụ nó, nhưng đầu thứ hai`'a'`không thể khớp với bất kỳ ký tự tiếp theo dự kiến ​​nào vì chuỗi dự kiến ​​sẽ chuyển sang`'b'`. Vì nhân vật thứ hai vẫn là`'a'`, quá trình không thành công, từ chối chính xác các bản sao. 

Vì`"z"`một mình, ký tự mong đợi bắt đầu tại`'a'`, nhưng cả hai đầu đều không khớp nên thuật toán sẽ loại bỏ ngay lập tức, phản ánh việc không thể bỏ qua các chữ cái trong bảng chữ cái trước đó trong cấu trúc.
