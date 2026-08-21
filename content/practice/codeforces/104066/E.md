---
title: "CF 104066E - \u0421\u0430\u043c\u0430\u044f \u0441\u0442\u0440\u0430\u0448\u043d\u0430\u044f \u0438\u0441\u0442\u043e\u0440\u0438\u044f"
description: "Chúng ta được cung cấp một chuỗi các từ tạo thành một câu chuyện, trong đó các từ được phân tách bằng dấu cách và mỗi từ chỉ bao gồm các chữ cái Latinh viết thường. Toàn bộ câu chuyện có thể được xem như một chuỗi dài, nhưng khoảng trắng là các ký tự đặc biệt chia nó thành các ranh giới từ."
date: "2026-07-02T03:14:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104066
codeforces_index: "E"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 (\u0431\u0430\u0437\u043e\u0432\u0430\u044f \u0432\u0435\u0440\u0441\u0438\u044f)"
rating: 0
weight: 104066
solve_time_s: 45
verified: true
draft: false
---

[CF 104066E - \u0421\u0430\u043c\u0430\u044f \u0441\u0442\u0440\u0430\u0448\u043d\u0430\u044f \u0438\u0441\u0442\u043e\u0440\u0438\u044f](https://codeforces.com/problemset/problem/104066/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các từ tạo thành một câu chuyện, trong đó các từ được phân tách bằng dấu cách và mỗi từ chỉ bao gồm các chữ cái Latinh viết thường. Toàn bộ câu chuyện có thể được xem như một chuỗi dài, nhưng khoảng trắng là các ký tự đặc biệt chia nó thành các ranh giới từ. 

Nhiệm vụ là trả lời các truy vấn về các vị trí bên trong chuỗi kết hợp này. Mỗi truy vấn cung cấp một chỉ mục ký tự chung trong chuỗi câu chuyện đầy đủ (bao gồm các chữ cái nhưng thường bỏ qua khoảng trắng cho mục đích lập chỉ mục trong mô hình truy vấn được mô tả trong câu lệnh vấn đề) và chúng ta phải xác định hai điều: từ nào chứa ký tự đó và vị trí của ký tự đó bên trong từ đó. 

Vì vậy, phép biến đổi cốt lõi là từ một chỉ mục tuyến tính trong biểu diễn được nối thành một cặp tọa độ: chỉ mục từ và vị trí trong từ đó. 

Các ràng buộc biểu thị tối đa 10^5 từ và tối đa 5·10^5 truy vấn, với tổng độ dài ký tự tối đa là 10^6. Điều này ngay lập tức ngụ ý rằng bất kỳ lần quét tuyến tính nào trên mỗi truy vấn đối với các từ hoặc ký tự đều quá chậm. Một giải pháp phải xử lý trước cấu trúc một lần và trả lời từng truy vấn theo thời gian không đổi hoặc logarit. Quét tuyến tính cho mỗi truy vấn sẽ đạt tới 5·10^11 thao tác trong trường hợp xấu nhất, điều này không khả thi trong giới hạn 1 giây. 

Một sai lầm ngây thơ nhưng phổ biến là coi khoảng trắng như các ký tự bình thường và quên rằng việc lập chỉ mục sẽ nhảy qua chúng. Một vấn đề nhỏ khác là lập chỉ mục riêng biệt khi ánh xạ tổng tiền tố thành các phân đoạn, đặc biệt khi truy vấn chạm chính xác đến ranh giới giữa các từ. 

Các trường hợp đặc biệt xuất hiện khi các từ có độ dài bằng 1 hoặc khi tất cả các từ đều rất ngắn, tạo ra các ranh giới thường xuyên. Ví dụ, nếu câu chuyện là`"a b c"`, bản đồ chỉ số toàn cầu chặt chẽ và các điều kiện biên rất quan trọng. Truy vấn trỏ chính xác vào ký tự cuối cùng của từ không được tràn sang từ tiếp theo. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: nối toàn bộ câu chuyện thành một chuỗi, sau đó quét về phía trước với mỗi truy vấn cho đến khi chúng ta đạt đến ký tự thứ k. Mặc dù đúng nhưng điều này yêu cầu O(n) hoạt động cho mỗi truy vấn trong trường hợp xấu nhất, vì việc định vị một vị trí yêu cầu duyệt qua từ đầu hoặc lặp qua các từ cho đến khi tìm thấy phân đoạn chính xác. Với tối đa 5·10^5 truy vấn và tổng chiều dài lên tới 10^6, điều này biến thành việc quét lặp đi lặp lại cùng một cấu trúc tiền tố, dẫn đến công việc lặp đi lặp lại. 

Quan sát quan trọng là ranh giới từ là tĩnh. Khi chúng tôi biết tổng tiền tố của độ dài từ, mọi truy vấn sẽ trở thành tìm kiếm tiền tố đầu tiên vượt quá một vị trí nhất định. Điều này chuyển đổi vấn đề thành vị trí phạm vi lặp lại trong một mảng đơn điệu, có thể được giải quyết bằng tìm kiếm nhị phân hoặc đơn giản hơn bằng cách quét hai con trỏ nếu các truy vấn được xử lý theo thứ tự. 

Vì tổng kích thước chỉ là 10^6 nên tổng tiền tố cộng với tìm kiếm nhị phân cho mỗi truy vấn là đủ và đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force cho mỗi truy vấn | O(n·m) | O(n) | Quá chậm | 
| Tổng tiền tố + Tìm kiếm nhị phân | O(n + m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi chuỗi từ thành cấu trúc tiền tố mô tả nơi mỗi từ bắt đầu và kết thúc trong chuỗi câu chuyện phẳng không có khoảng trắng. 

Chúng tôi duy trì một mảng`pref`, Ở đâu`pref[i]`lưu trữ tổng số ký tự trong đầu tiên`i`từ. 

Khi đó mỗi truy vấn sẽ trở thành một bài toán tìm kiếm trên mảng này. 

1. Xây dựng một dãy độ dài từ. Mỗi từ đóng góp số ký tự của nó. 
2. Xây dựng tổng tiền tố sao cho`pref[i]`là tổng số ký tự trong từ`[1..i]`. Điều này tạo ra một dãy tăng đơn điệu. 
3. Đối với vị trí truy vấn`x`, tìm chỉ số nhỏ nhất`i`như vậy`pref[i] >= x`. Điều này xác định từ có chứa ký tự. 
4. Sau khi tìm thấy từ đó, hãy tính vị trí bên trong từ đó như sau:`x - pref[i-1]`. 

Tính đúng đắn của bước 3 phụ thuộc vào thực tế là tiền tố tổng hợp phân chia chuỗi toàn cục thành các phân đoạn rời rạc liền kề tương ứng chính xác với các từ. 

Tìm kiếm nhị phân trực tiếp là đủ vì`pref`đang gia tăng nghiêm trọng. 

### Tại sao nó hoạt động 

Mỗi từ chiếm một khoảng liền kề trong chuỗi câu chuyện được làm phẳng và các khoảng này không trùng nhau. Mảng tiền tố mã hóa điểm cuối bên phải của mỗi khoảng. Việc định vị tiền tố đầu tiên vượt quá hoặc bằng chỉ mục truy vấn tương đương với việc tìm khoảng chứa chỉ mục đó. Vì các khoảng liền kề nhau và có thứ tự nên ánh xạ này là duy nhất và mang tính xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    words = input().split()

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + len(words[i - 1])

    def find_word(x):
        lo, hi = 1, n
        while lo < hi:
            mid = (lo + hi) // 2
            if pref[mid] >= x:
                hi = mid
            else:
                lo = mid + 1
        return lo

    for _ in range(m):
        x = int(input())
        i = find_word(x)
        prev = pref[i - 1]
        print(i, x - prev)

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt rõ ràng việc xử lý tiền xử lý và xử lý truy vấn. Mảng tiền tố được tạo một lần trong O(n). Mỗi truy vấn sử dụng tìm kiếm nhị phân trên ranh giới từ. 

chức năng`find_word`được viết cẩn thận để tránh lỗi sai sót một. điều kiện`pref[mid] >= x`đảm bảo chúng tôi đến từ đầu tiên có ranh giới bên phải không nằm trước chỉ mục truy vấn. 

Phép trừ`x - pref[i - 1]`chuyển đổi vị trí chung thành phần bù cục bộ bên trong từ đã chọn. 

## Ví dụ đã hoạt động 

Hãy xem xét một câu chuyện nhỏ với lời nói: 

đầu vào:```
3 3
hell spirits fear
1
7
10
```Ở đây tổng tiền tố là: 

| tôi | từ | chiều dài | trước[i] | 
| --- | --- | --- | --- | 
| 0 | - | - | 0 | 
| 1 | địa ngục | 4 | 4 | 
| 2 | tinh thần | 7 | 11 | 
| 3 | sợ hãi | 4 | 15 | 

Dấu vết truy vấn: 

| x | lo | xin chào | quyết định giữa chừng | tìm thấy từ i | vị trí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1-3 | ... | hội tụ về 1 | 1 | 1 | 
| 7 | 1-3 | pref[2]=11 ≥7 → từ 2 | 2 | 7 - 4 = 3 | | 
| 10 | ... | pref[2]=11 ≥10 → từ 2 | 2 | 10 - 4 = 6 | | 

Bảng này cho thấy cách tìm kiếm nhị phân thu hẹp đến phân đoạn tiền tố chính xác. Mỗi kết quả xác nhận rằng các chỉ số được diễn giải liên quan đến ranh giới từ thay vì nối đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m log n) | việc xây dựng tiền tố là tuyến tính, mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian | O(n) | mảng tiền tố lưu trữ một giá trị cho mỗi từ | 

Các ràng buộc cho phép tổng cộng tối đa 10^6 ký tự và 5·10^5 truy vấn, do đó, tìm kiếm logarit cho mỗi truy vấn có thể dễ dàng đủ nhanh. Việc sử dụng bộ nhớ bị chi phối bởi việc lưu trữ độ dài từ và tổng tiền tố. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()

    n, m = map(int, sys.stdin.readline().split())
    words = sys.stdin.readline().split()

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + len(words[i - 1])

    def find_word(x):
        lo, hi = 1, n
        while lo < hi:
            mid = (lo + hi) // 2
            if pref[mid] >= x:
                hi = mid
            else:
                lo = mid + 1
        return lo

    for _ in range(m):
        x = int(sys.stdin.readline())
        i = find_word(x)
        output.write(f"{i} {x - pref[i - 1]}\n")

    return output.getvalue()

# sample-like test
assert run("3 3\nhell spirits fear\n1\n7\n10\n") == "1 1\n2 3\n2 6\n"

# minimum size
assert run("1 3\na\n1\n1\n1\n") == "1 1\n1 1\n1 1\n"

# boundary test
assert run("2 2\naa b\n2\n3\n") == "1 2\n2 1\n"

# all equal lengths
assert run("4 4\na b c d\n1\n2\n3\n4\n") == "1 1\n2 1\n3 1\n4 1\n"

# long word
assert run("1 1\nabcde\n5\n") == "1 5\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lặp lại một từ | 1 1 | tính đúng đắn của trường hợp tối thiểu | 
| chuyển tiếp ranh giới | chuyển từ đúng | từng cái một ở các cạnh | 
| từ có độ dài bằng nhau | ánh xạ tuần tự | tính chính xác của tiền tố | 
| truy vấn từ dài | vị trí cuối cùng | xử lý ranh giới cuối | 

## Vỏ cạnh 

Trường hợp quan trọng là khi truy vấn đến chính xác ký tự cuối cùng của từ. Trong một câu chuyện như`"abc def"`, tổng tiền tố là`[3, 6]`. Một truy vấn`x = 3`phải ánh xạ tới từ đầu tiên, không phải từ thứ hai. điều kiện`pref[mid] >= x`đảm bảo rằng sự bình đẳng được giải quyết đối với từ ranh giới bên trái, ngăn chặn tình trạng tràn sang từ tiếp theo. 

Một trường hợp tinh vi khác là khi tất cả các từ đều có độ dài bằng 1. Khi đó tổng tiền tố hoàn toàn bị ràng buộc.`1, 2, 3, ...`và tìm kiếm nhị phân vẫn phải ánh xạ chính xác từng truy vấn tới từ chính xác của nó. Thuật toán xử lý việc này vì mỗi chỉ mục khớp với một giá trị tiền tố duy nhất. 

Cuối cùng, khi chỉ có một từ, mọi truy vấn đều ánh xạ tới từ 1 và vị trí giống hệt với chỉ mục truy vấn. Việc triển khai không yêu cầu cách viết hoa đặc biệt vì mảng tiền tố sẽ suy biến chính xác thành`[0, len(word)]`.
