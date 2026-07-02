---
title: "CF 104199E - \u041d\u0435 \u0432\u0441\u0435 \u0441\u043f\u0435\u0446\u0438\u0438 \u043e\u0434\u0438\u043d\u0430\u043a\u043e\u0432\u043e \u043f\u043e\u043b\u0435\u0437\u043d\u044b"
description: "Có $n$ loại gia vị khác nhau trong nhà bếp, mỗi loại được xác định bằng một tên. Một món ăn hàng ngày được chế biến bằng cách chọn chính xác $m$ các loại gia vị riêng biệt, nhưng chúng tôi không biết những loại gia vị nào đã được chọn."
date: "2026-07-02T18:00:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104199
codeforces_index: "E"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 18-02-23"
rating: 0
weight: 104199
solve_time_s: 83
verified: true
draft: false
---

[CF 104199E - \u041d\u0435 \u0432\u0441\u0435 \u0441\u043f\u0435\u0446\u0438\u0438 \u043e\u0434\u0438\u043d\u0430\u043a\u043e\u0432\u043e \u043f\u043e\u043b\u0435\u0437\u043d\u044b](https://codeforces.com/problemset/problem/104199/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có$n$các loại gia vị khác nhau trong nhà bếp, mỗi loại được xác định bằng một tên. Một món ăn hàng ngày được chuẩn bị bằng cách lựa chọn chính xác$m$các loại gia vị riêng biệt, nhưng chúng tôi không biết loại nào được chọn. 

Chúng ta được biết một quan sát quan trọng: một đầu bếp phụ, người bị dị ứng với một loạt món ăn đã biết.$k$gia vị, đã nếm thử món ăn và không bị bất kỳ phản ứng dị ứng nào. Điều này hạn chế thành phần món ăn chưa biết: không có món nào được chọn$m$gia vị có thể thuộc bộ dị ứng của người trợ giúp. 

Vì vậy, một cách hiệu quả, món ăn là một tập hợp con chưa biết kích thước$m$, nhưng chỉ từ những loại gia vị an toàn cho người trợ giúp. 

Bây giờ chúng tôi được trao$p$khách. Mỗi khách có danh sách dị ứng riêng. Đối với mỗi khách, chúng ta phải xác định những gì có thể kết luận về việc liệu món ăn có thể gây dị ứng cho họ hay không. Vì món ăn thực tế không được biết đến một cách duy nhất nên chúng tôi suy luận về tất cả các món ăn hợp lệ phù hợp với quan sát của người trợ giúp. 

Đối với mỗi khách, ba kết quả có thể xảy ra. Nếu mọi món ăn hợp lệ đều tránh được tất cả các chất gây dị ứng thì câu trả lời là “KHÔNG”, nghĩa là món ăn đó được đảm bảo an toàn cho họ. Nếu mọi món ăn hợp lệ nhất thiết phải chứa ít nhất một trong số các chất gây dị ứng thì câu trả lời là “CÓ”, nghĩa là món ăn đó chắc chắn sẽ gây ra phản ứng. Ngược lại, cả hai kết quả đều có thể xảy ra tùy thuộc vào cách thức chưa biết$m$-subset được chọn nên câu trả lời là “CÓ THỂ”. 

Những hạn chế$n \le 100$,$p \le 100$và các tập hợp chuỗi nhỏ gợi ý rằng chúng ta có thể đủ khả năng suy luận dựa trên tập hợp và thậm chí tính toán lại cho mỗi truy vấn mà không phải lo lắng về độ phức tạp tiệm cận ngoài việc đếm và băm đơn giản. 

Một điểm tinh tế là bài kiểm tra của người trợ giúp đã loại bỏ hoàn toàn một số loại gia vị khỏi việc xem xét. Bất kỳ loại gia vị nào trong danh sách dị ứng của họ đều được đảm bảo không xuất hiện trong món ăn, vì vậy những dị ứng của khách chỉ trùng với những loại gia vị đã loại bỏ sẽ trở nên không liên quan. 

Một cạm bẫy phổ biến khác là đối xử với từng vị khách một cách độc lập mà không phụ thuộc vào số lượng các loại gia vị hợp lệ. Món ăn không phải là bất kỳ tập hợp con nào về kích thước$m$từ tất cả$n$gia vị, nhưng chỉ từ những loại không nằm trong danh sách dị ứng của người trợ giúp. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là liệt kê mọi món ăn hợp lệ có thể có: trước tiên hãy lọc ra các loại gia vị gây dị ứng của người trợ giúp, sau đó tạo ra tất cả các kết hợp của$m$gia vị từ bộ còn lại và đối với mỗi khách, hãy kiểm tra xem liệu bất kỳ sự kết hợp nào trong số đó có nằm trong danh sách dị ứng của họ hay không. Điều này ngay lập tức trở nên không khả thi bởi vì ngay cả với$n = 100$, số tổ hợp$\binom{100}{50}$có quy mô lớn về mặt thiên văn và chúng tôi sẽ lặp lại việc kiểm tra cho tối đa 100 khách. 

Quan sát quan trọng là chúng ta không bao giờ cần phải xây dựng món ăn một cách rõ ràng. Tất cả các món ăn hợp lệ chỉ đơn giản là tất cả$m$-các tập con của một vũ trụ có kích thước thu nhỏ cố định$n - k$. Đối với một vị khách nhất định, chỉ có hai đặc tính cấu trúc quan trọng: có bao nhiêu loại gia vị được phép tồn tại mà không có trong bộ dị ứng của họ và liệu có loại gia vị dị ứng nào của họ có đủ điều kiện xuất hiện trong món ăn hay không. 

Khi chúng tôi kết hợp bộ dị ứng của từng khách với vũ trụ an toàn, mọi thứ sẽ giảm xuống thành một phép đếm đơn giản. Chúng tôi chỉ theo dõi có bao nhiêu loại gia vị “nguy hiểm nhưng vẫn có thể xảy ra” đối với vị khách đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force tất cả các món ăn hợp lệ | Số mũ trong$n$| Cao | Quá chậm | 
| Đặt đếm giao lộ |$O(n + p \cdot n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Hãy để gia vị gây dị ứng của người trợ giúp xác định một bộ bị cấm. Vũ trụ hợp lệ của các loại gia vị là tất cả mọi thứ ngoại trừ những món đồ bị cấm này. 

Đối với mỗi khách, chúng tôi phân loại các loại gia vị trong danh sách dị ứng của họ thành hai loại: những loại vẫn còn tồn tại trong phạm vi hợp lệ và những loại đã bị loại trừ bởi sự ràng buộc của người trợ giúp. Chỉ có loại đầu tiên ảnh hưởng đến câu trả lời. 

1. Xây dựng ánh xạ từ tên gia vị đến chỉ mục để chúng ta có thể làm việc hiệu quả với tập hợp thay vì chuỗi. Điều này cho phép kiểm tra tư cách thành viên liên tục. 
2. Đọc bộ tài liệu về dị ứng dành cho người trợ giúp và đánh dấu tất cả các loại gia vị này là bị cấm. Vũ trụ hợp lệ là tất cả các loại gia vị không có trong bộ này và kích thước của nó là$N' = n - k$. 
3. Đối với mỗi khách, hãy đếm xem có bao nhiêu loại gia vị gây dị ứng của họ vẫn còn tồn tại trong vũ trụ hợp lệ. Gọi giá trị này$x$. Điều này thể hiện số lượng gia vị thực sự có thể xuất hiện trong món ăn và vẫn kích hoạt chúng. 
4. Tính xem còn lại bao nhiêu loại gia vị an toàn trong vũ trụ đối với vị khách này, đó là$N' - x$. Đây là những loại gia vị có thể dùng trong món ăn mà không gây dị ứng cho chúng. 
5. Nếu$x = 0$, thì không chất gây dị ứng nào của khách có thể xuất hiện trong bất kỳ món ăn hợp lệ nào. Mọi món ăn hợp lệ đều tránh chúng, vì vậy câu trả lời là “KHÔNG”. 
6. Ngược lại, nếu$N' - x < m$, thì không thể chọn được$m$gia vị mà không bao gồm ít nhất một trong các chất gây dị ứng của chúng. Mỗi món ăn hợp lệ đều phải chứa thứ gì đó nguy hiểm cho chúng, vì vậy câu trả lời là “CÓ”. 
7. Trong tất cả các trường hợp còn lại đều tồn tại cả lựa chọn hoàn toàn an toàn và lựa chọn nguy hiểm nên câu trả lời là “CÓ THỂ”. 

### Tại sao nó hoạt động 

Tất cả các món ăn hợp lệ đều đồng nhất$m$-tập hợp con của một vũ trụ có kích thước cố định$N'$. Cách duy nhất để khách tránh được phản ứng dị ứng với một món ăn cụ thể là nếu tập hợp con được chọn tránh được tất cả$x$về các chất gây dị ứng vẫn có thể xảy ra. Việc này có luôn luôn khả thi hay không chỉ phụ thuộc vào việc có ít nhất$m$gia vị không gây dị ứng có sẵn. Nếu có ít hơn$m$, mỗi tập hợp con phải bao gồm ít nhất một chất gây dị ứng; nếu không có thì mọi tập hợp con sẽ tránh chúng; mặt khác cả hai công trình đều tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
k = int(input())

all_spices = set()
bad = set()

for _ in range(k):
    bad.add(input().strip())

p = int(input())

for _ in range(p):
    ni = int(input())
    guest = set()
    for _ in range(ni):
        guest.add(input().strip())

    # compute intersection with helper-banned set
    # and compute effective dangerous spices
    x = 0
    for s in guest:
        if s not in bad:
            x += 1

    # actually x = |guest ∩ safe|, but we want safe allergens count
    # recompute properly:
    x = len([s for s in guest if s not in bad])

    safe_pool_size = n - k
    safe_non_guest = safe_pool_size - x

    if x == 0:
        print("NO")
    elif safe_non_guest < m:
        print("YES")
    else:
        print("MAYBE")
```Việc triển khai bắt đầu bằng cách đọc danh sách dị ứng của người trợ giúp và coi nó như một bộ lọc bị cấm. Mỗi khách được xử lý độc lập bằng cách đếm xem có bao nhiêu chất gây dị ứng của họ tồn tại qua bộ lọc này. 

Biến$x$thể hiện số lượng chất gây dị ứng của khách vẫn đủ điều kiện xuất hiện trong món ăn. Một khi đã biết được điều đó, phần còn lại của lý do sẽ chuyển thành một so sánh đơn giản giữa số lượng gia vị an toàn có thể sử dụng được và kích thước món ăn cần thiết.$m$. 

Thứ tự phân nhánh quan trọng. Trường hợp “KHÔNG” phải được kiểm tra trước vì nó thể hiện cấu trúc không thể nào khiến khách bị ảnh hưởng, bất kể tổ hợp. Trường hợp “CÓ” xuất phát từ một cuộc tranh luận về nhóm an toàn còn lại. Mọi thứ khác là trường hợp hỗn hợp. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu được cung cấp. 

### Mẫu 1 

| Khách | Khách gây dị ứng | x (trong nhóm an toàn) | safe_pool_size - x | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | hạt tiêu | 0 | 4 | KHÔNG | 
| 2 | thì là, cỏ cà ri, chanh | 1 | 3 | CÓ | 
| 3 | imbir, vôi | 2 | 2 | CÓ THỂ | 

Người trợ giúp loại bỏ`pepper`,`imbir`,`cumin`, chỉ để lại những gia vị xác định tất cả các món ăn hợp lệ. Đối với mỗi khách, chúng tôi kiểm tra xem chất gây dị ứng của họ có tồn tại trong vũ trụ thu nhỏ này hay không và liệu có tránh chúng khi chọn không$m = 3$gia vị vẫn có thể. 

Vị khách đầu tiên không còn chất gây dị ứng nào trong vũ trụ hợp lệ nên họ được đảm bảo an toàn. Vị khách thứ hai còn lại quá ít lựa chọn thay thế an toàn để tránh tất cả các chất gây dị ứng, gây ra phản ứng. Khách thứ ba nằm ở giữa, nơi có thể xây dựng cả công trình an toàn và không an toàn tùy thuộc vào tập hợp con hợp lệ nào được chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(p \cdot n)$| Mỗi khách xử lý tối đa$n$kiểm tra gia vị đối với bộ của người trợ giúp | 
| Không gian |$O(n)$| Kho đựng bộ gia vị và bản đồ | 

Những giới hạn$n, p \le 100$làm điều này nhanh chóng thoải mái. Ngay cả một giao điểm tập hợp dựa trên chuỗi đơn giản cũng chạy ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    k = int(input())
    bad = set(input().strip() for _ in range(k))

    p = int(input())
    out = []
    for _ in range(p):
        ni = int(input())
        guest = set(input().strip() for _ in range(ni))

        x = len([s for s in guest if s not in bad])
        safe_pool = n - k
        safe_non_guest = safe_pool - x

        if x == 0:
            out.append("NO")
        elif safe_non_guest < m:
            out.append("YES")
        else:
            out.append("MAYBE")

    return "\n".join(out)

# provided sample
assert run("""7 3
3
pepper
imbir
cumin
3
1
pepper
3
cumin
fenugreek
lime
2
imbir
lime
""") == """NO
YES
MAYBE"""

# all safe trivial
assert run("""3 2
1
a
1
0
""") == "NO"

# forced YES
assert run("""4 3
1
a
1
2
b
c
""") == "YES"

# MAYBE case
assert run("""5 2
1
a
1
1
b
""") == "MAYBE"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | hỗn hợp | chính xác hoàn toàn trên cả ba kết quả | 
| nhỏ KHÔNG | KHÔNG | xử lý giao lộ vắng với vũ trụ an toàn | 
| buộc CÓ | CÓ | chuồng chim buộc đưa chất gây dị ứng vào | 
| trường hợp hỗn hợp | CÓ THỂ | tồn tại của cả hai công trình hợp lệ | 

## Vỏ cạnh 

Khi một vị khách có tất cả các chất gây dị ứng có trong danh sách cấm của người trợ giúp, toàn bộ danh sách chất gây dị ứng của họ sẽ bị loại khỏi danh sách xem xét một cách hiệu quả. Trong tình huống đó, mọi món ăn hợp lệ sẽ tự động tránh chúng vì vũ trụ món ăn không bao giờ chứa bất kỳ loại gia vị kích hoạt nào. Thuật toán nắm bắt điều này thông qua$x = 0$, ngay lập tức tạo ra “KHÔNG” mà không cần bất kỳ lý luận tổ hợp nào. 

Khi số lượng gia vị an toàn bên ngoài bộ dị ứng của khách quá ít để lấp đầy kích thước món ăn$m$, mọi lựa chọn hợp lệ phải bao gồm ít nhất một trong các chất gây dị ứng của chúng. Sự tính toán$N' - x < m$xác định trực tiếp tình huống bắt buộc đưa vào này, đảm bảo "CÓ" ngay cả khi các chất gây dị ứng được phân phối thưa thớt. 

Khi cả hai điều kiện đều không thành công, sẽ có đủ tính linh hoạt để tạo ra một món ăn hợp lệ tránh hoàn toàn cho khách và một món ăn khác có ít nhất một chất gây dị ứng. Thuật toán phân loại điều này là “CÓ THỂ”, phản ánh sự cùng tồn tại của cả hai cấu trúc tổ hợp khả thi trong vũ trụ bị ràng buộc.
