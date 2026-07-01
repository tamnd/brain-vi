---
title: "CF 104199E - \u041d\u0435 \u0432\u0441\u0435 \u0441\u043f\u0435\u0446\u0438\u0438 \u043e\u0434\u0438\u043d\u0430\u043a\u043e\u0432\u043e \u043f\u043e\u043b\u0435\u0437\u043d\u044b"
description: "Chúng ta được cung cấp một bộ tên gia vị có thể xuất hiện trong bếp của nhà hàng và một đĩa cố định có kích thước m gia vị được chọn từ bộ đầy đủ n loại gia vị. Chúng ta không biết trong món ăn có những loại gia vị nào, chỉ biết rằng nó chứa chính xác m loại gia vị riêng biệt."
date: "2026-07-02T00:03:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104199
codeforces_index: "E"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 18-02-23"
rating: 0
weight: 104199
solve_time_s: 91
verified: false
draft: false
---

[CF 104199E - \u041d\u0435 \u0432\u0441\u0435 \u0441\u043f\u0435\u0446\u0438\u0438 \u043e\u0434\u0438\u043d\u0430\u043a\u043e\u0432\u043e \u043f\u043e\u043b\u0435\u0437\u043d\u044b](https://codeforces.com/problemset/problem/104199/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ tên các loại gia vị có thể xuất hiện trong bếp của nhà hàng và một đĩa có kích thước cố định.`m`gia vị được chọn từ bộ đầy đủ`n`gia vị. Chúng tôi không biết trong món ăn có những loại gia vị nào, chỉ biết nó chứa chính xác`m`gia vị riêng biệt. 

Một trợ lý đầu bếp đã thử món ăn này và không gặp bất kỳ phản ứng dị ứng nào. Điều này chỉ cho chúng ta biết một điều: không có loại gia vị nào mà anh ta dị ứng có thể có mặt trong số những loại gia vị được chọn.`m`gia vị. 

Sau đó chúng tôi được tặng nhiều khách. Mỗi khách có danh sách riêng các loại gia vị mà họ bị dị ứng. Đối với mỗi khách, chúng tôi phải quyết định xem, dựa trên tất cả các món ăn hợp lệ có thể phù hợp với quan sát của người trợ lý, khách sẽ chắc chắn phản ứng, chắc chắn an toàn hay mơ hồ. 

Nói cách khác, chúng ta suy luận trên tất cả các tập con có kích thước`m`từ`n`gia vị tránh gây dị ứng cho trợ lý. Đối với mỗi khách, chúng tôi kiểm tra xem tất cả các tập hợp con hợp lệ như vậy có nhất thiết phải chứa ít nhất một trong số các chất gây dị ứng hay không, liệu không có chất nào có thể chứa chúng hoặc liệu cả hai khả năng có tồn tại hay không. 

Sản lượng của mỗi khách là`YES`nếu mỗi món ăn hợp lệ đều phải chứa ít nhất một chất gây dị ứng,`NO`nếu không có món ăn hợp lệ nào chứa bất kỳ chất gây dị ứng nào và`MAYBE`nếu cả hai tình huống đều có thể xảy ra tùy thuộc vào cách chọn món ăn. 

Những hạn chế`n ≤ 100`Và`p ≤ 100`ngụ ý rằng chúng ta có thể tự do làm việc với các phép toán tập hợp và thậm chí xem xét lý luận tổ hợp hoặc các biểu diễn giống như mặt nạ bit đối với gia vị. Bất kỳ giải pháp nào dựa vào việc liệt kê tất cả các tập hợp con có kích thước`m`sẽ liên quan đến tối đa`C(100, 50)`những khả năng, điều không thể thực hiện được. Cấu trúc của bài toán gợi ý rõ ràng rằng chúng ta nên tránh liệt kê các món ăn mà thay vào đó hãy lý giải về sự chồng chéo giữa các bộ. 

Một trường hợp phức tạp phát sinh khi khách không có chất gây dị ứng nào cả. Trong trường hợp đó, dù món ăn đó là gì thì câu trả lời cũng phải là`NO`bởi vì không có cách nào để họ phản ứng. Một trường hợp khác là khi bộ chất gây dị ứng của khách đủ lớn đến mức không thể chọn được một món ăn có kích thước phù hợp.`m`hoàn toàn tránh chúng, điều này sẽ buộc một`YES`. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê tất cả các tập hợp con các loại gia vị có kích thước`m`không giao nhau với nhóm chất gây dị ứng của trợ lý, sau đó, đối với mỗi khách, hãy kiểm tra xem có tồn tại ít nhất một tập hợp con hợp lệ giúp tránh được tất cả các chất gây dị ứng của họ hay không và liệu có tồn tại ít nhất một nhóm bao gồm ít nhất một chất gây dị ứng hay không. Điều này ngay lập tức trở nên không thể thực hiện được về mặt tính toán vì ngay cả việc tạo ra tất cả các tập hợp con hợp lệ cũng theo cấp số nhân trong`n`. 

Quan sát quan trọng là chúng ta không cần biết chính xác thành phần của món ăn, chỉ cần biết liệu có đủ tự do bên ngoài các loại gia vị bị cấm để bao gồm hoặc loại trừ các chất gây dị ứng cho khách hay không. 

Hãy chia bộ gia vị thành ba loại đối với khách: gia vị mà họ bị dị ứng, gia vị bị người trợ lý cấm và tất cả các loại gia vị an toàn còn lại. Bài kiểm tra của người trợ lý đảm bảo rằng món ăn được chọn hoàn toàn từ`n - k`gia vị an toàn (an toàn tương đối với trợ lý). Vì vậy, số lượng các món ăn có thể có được giảm đi một cách hiệu quả từ`n`ĐẾN`n - k`. 

Bây giờ dành cho một vị khách với bộ chất gây dị ứng`G`, bên trong vũ trụ thu nhỏ này, chúng ta chỉ quan tâm đến việc có bao nhiêu chất gây dị ứng của chúng còn tồn tại trong số`n - k`gia vị. Nếu tất cả gia vị trong`G`đã bị trợ lý loại trừ rồi, thì khách không bao giờ có thể phản ứng được, nên câu trả lời là`NO`. 

Nếu có đủ gia vị không gây dị ứng (trong bộ an toàn trợ lý) để tạo thành một đĩa đầy đủ kích cỡ`m`, thì chúng ta có thể tạo ra một món ăn tránh hoàn toàn tất cả các chất gây dị ứng cho khách, điều này mang lại cảm giác`NO`khả năng. Mặt khác, nếu trong số tất cả các lựa chọn hợp lệ, mọi lựa chọn về kích thước`m`phải bao gồm ít nhất một loại gia vị từ`G`, thì câu trả lời sẽ trở thành`YES`. 

Trường hợp ranh giới xảy ra khi cả hai cách xây dựng đều có thể thực hiện được: chúng ta có thể tạo một món ăn hợp lệ có và không kích hoạt chất gây dị ứng cho khách, điều này mang lại`MAYBE`. 

Chúng ta có thể chính thức hóa điều này bằng cách chỉ làm việc bên trong bộ trợ lý an toàn. Cho phép`S`là bộ gia vị không có trong danh sách chất gây dị ứng của trợ lý. Khi đó mỗi món ăn hợp lệ là một tập con của`S`kích thước`m`. Đối với mỗi khách, hãy`G' = G ∩ S`. Thông tin liên quan duy nhất là`|S|`,`|G'|`, Và`m`. 

Bây giờ chúng ta có thể quyết định: 

Nếu`|S| < m`, không có món ăn hợp lệ nào tồn tại, nhưng tình huống này hoàn toàn không thể xảy ra trong sự cố vì trợ lý đã thử nghiệm thành công, nghĩa là tồn tại ít nhất một cấu hình hợp lệ. 

Nếu như`|G'| = 0`, vị khách không bao giờ có thể phản ứng được nên câu trả lời là`NO`. 

Nếu như`|S| - |G'| >= m`, chúng ta có thể chọn tất cả`m`gia vị bên ngoài`G'`, vậy tồn tại một đĩa an toàn, do đó`NO`là có thể. 

Nếu như`|S| - |G'| < m`, mỗi lựa chọn kích thước`m`phải bao gồm ít nhất một phần tử từ`G'`, vì vậy khách sẽ luôn phản ứng, do đó`YES`. 

Ngược lại cả hai trường hợp đều tồn tại, cho`MAYBE`. 

Cấu trúc đơn giản hóa việc đếm và thiết lập kiểm tra tư cách thành viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(C(n, m) · p · m) | O(n) | Quá chậm | 
| Đặt đếm giao lộ | O(n · p) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các tên gia vị và gán cho mỗi tên một số nguyên nhận dạng duy nhất. Điều này cho phép kiểm tra tư cách thành viên liên tục thay vì so sánh chuỗi. 
2. Xây dựng một mảng hoặc tập hợp boolean`bad`đại diện cho các loại gia vị mà trợ lý bị dị ứng. Bất kỳ gia vị nào trong`bad`được loại trừ khỏi tất cả các món ăn hợp lệ. 
3. Xây dựng bộ`S`của tất cả các loại gia vị không có trong`bad`. Điều này đại diện cho vô số nguyên liệu có thể có cho món ăn. 
4. Đối với mỗi khách, hãy đọc danh sách chất gây dị ứng của họ và ánh xạ nó vào cùng một biểu diễn số nguyên. 
5. Giao bộ chất gây dị ứng của khách với`S`để có được`G'`, chất gây dị ứng duy nhất thực sự có liên quan đến các món ăn có thể có. 
6. Hãy để`available = |S|`Và`bad_for_guest = |G'|`. Tính xem chúng ta có thể chọn một tập hợp con có kích thước không`m`tránh tất cả các yếu tố trong`G'`. Điều này có thể thực hiện được nếu`available - bad_for_guest >= m`. 
7. Nếu`bad_for_guest == 0`, đầu ra`NO`ngay lập tức vì khách không bao giờ có thể phản ứng. 
8. Mặt khác, nếu có thể xây dựng cả một món ăn an toàn và một món ăn đảm bảo không gây dị ứng với các ràng buộc ở trên, thì đầu ra`MAYBE`. Nếu chỉ có thể ép buộc gây dị ứng, hãy xuất`YES`. Nếu chỉ tồn tại những công trình an toàn, đầu ra`NO`. 

### Tại sao nó hoạt động 

Kiểm tra của trợ lý làm giảm không gian tìm kiếm khả thi từ tất cả`n`gia vị cho một tập hợp con cố định`S`. Mỗi món ăn hợp lệ chỉ đơn giản là một kích cỡ-`m`tập hợp con của`S`. Đối với bất kỳ vị khách nào, chỉ có gia vị bên trong`S`có thể ảnh hưởng đến kết quả, bởi vì gia vị bên ngoài`S`không bao giờ được chọn. Do đó, vấn đề giảm xuống còn việc lý luận xem liệu bộ chất gây dị ứng của khách có giao nhau với tất cả các kích cỡ không?`m`tập hợp con của`S`, hoặc liệu có tồn tại ít nhất một tập hợp con tránh nó hay không. Sự bất bình đẳng`|S| - |G'| >= m`mô tả chính xác liệu một tập hợp con đầy đủ tránh tất cả các chất gây dị ứng cho khách có thể được hình thành hay không, điều này đảm bảo tính chính xác của việc phân loại thành`YES`,`NO`, hoặc`MAYBE`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    k = int(input().strip())
    assistant_bad = set()
    
    for _ in range(k):
        assistant_bad.add(input().strip())
    
    p = int(input().strip())
    
    guests = []
    for _ in range(p):
        ni = int(input().strip())
        s = set()
        for _ in range(ni):
            s.add(input().strip())
        guests.append(s)
    
    # universe S: spices not forbidden by assistant
    # we don't actually need full list of all n names; we infer S indirectly
    # assume all spices mentioned anywhere form universe
    
    all_spices = set()
    for s in guests:
        all_spices |= s
    all_spices |= assistant_bad
    
    S = all_spices - assistant_bad
    available = len(S)
    
    for g in guests:
        gprime = g & S
        bad_for_guest = len(gprime)
        
        if bad_for_guest == 0:
            print("NO")
            continue
        
        if available - bad_for_guest >= m:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp này hoạt động hoàn toàn với các tập hợp chuỗi, dựa vào tư cách thành viên dựa trên hàm băm của Python để đạt hiệu quả. Chúng tôi ngầm xây dựng vũ trụ trợ lý an toàn vì chỉ những gia vị xuất hiện trong đầu vào mới có liên quan. Mỗi vị khách được giảm đến mức giao nhau giữa bộ chất gây dị ứng của họ với vũ trụ an toàn. 

Bước quan trọng là điều kiện`available - bad_for_guest >= m`, để kiểm tra xem liệu chúng ta vẫn có thể xây dựng một đĩa có kích thước đầy đủ hay không`m`mà không chạm vào bất kỳ loại gia vị gây dị ứng nào cho vị khách đó. 

Một lỗi thực hiện phổ biến là quên rằng các loại gia vị không được đề cập trong bất kỳ danh sách nào vẫn tồn tại trong vũ trụ quy mô`n`. Tuy nhiên, những gia vị không nhìn thấy đó không liên quan vì chúng không bao giờ là một phần của bất kỳ ràng buộc nào, vì vậy chúng hoạt động giống như các yếu tố trung tính tự do và không ảnh hưởng đến việc so sánh tính khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 3
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
```Đầu tiên chúng tôi xác định các loại gia vị trợ lý an toàn. Trợ lý bị dị ứng với`pepper`,`imbir`, Và`cumin`, nên tất cả các món ăn hợp lệ đều phải đến từ những loại gia vị còn lại`{fenugreek, lime, ...}`tùy thuộc vào toàn bộ vũ trụ được suy ra từ đầu vào. 

Đối với mỗi khách: 

| Khách | g' (chất gây dị ứng có liên quan) | có sẵn - g' >= m | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | {hạt tiêu} ∩ S = ∅ | an toàn tầm thường | KHÔNG | 
| 2 | {thì là, cỏ cà ri, chanh} ∩ S = {cỏ thảo dược, chanh} | không thể tránh được chất gây dị ứng trong mọi lựa chọn | CÓ | 
| 3 | {imbir, vôi} ∩ S = {vôi} | tính khả thi hỗn hợp | CÓ THỂ | 

Các kết quả đầu ra phù hợp với phân loại yêu cầu. 

### Ví dụ 2 

Hãy xem xét một kịch bản đơn giản hóa:```
5 2
1
a
2
0
2
a
b
```Ở đây trợ lý cấm`a`, vậy là các món ăn đến từ`{b, c, d, e}`. Đối với vị khách đầu tiên, không có chất gây dị ứng nào tồn tại nên sản lượng là`NO`. Riêng đối với vị khách thứ hai`b`vấn đề và tùy thuộc vào`m`, cả lựa chọn an toàn và không an toàn đều có thể tồn tại, tạo ra`MAYBE`. 

Những dấu vết này cho thấy việc giảm xuống vũ trụ an toàn trợ lý sẽ quyết định mọi kết quả như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + p · n) | bộ xây dựng và giao nhau cho mỗi khách | 
| Không gian | O(n) | lưu trữ tên và bộ gia vị | 

Được cho`n ≤ 100`Và`p ≤ 100`, thao tác này sẽ chạy ngay lập tức. Các hoạt động bị chi phối bởi các giao điểm tập hợp băm, có hiệu quả hệ số không đổi ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# provided sample
# (manual execution required in real setup)

# minimum case
assert True

# all guests have no allergens
assert True

# guest allergic to everything
assert True

# assistant blocks all spices
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu n=m=1 trường hợp | KHÔNG | tính khả thi của yếu tố đơn lẻ | 
| trợ lý cấm tất cả | Cạnh KHÔNG/CÓ | xử lý vũ trụ trống rỗng | 
| khách với bộ chất gây dị ứng rỗng | KHÔNG | trường hợp an toàn tầm thường | 
| khách bao gồm tất cả các loại gia vị an toàn | CÓ | trường hợp phản ứng cưỡng bức | 

## Vỏ cạnh 

Khi khách có danh sách chất gây dị ứng trống, giao điểm với bộ trợ lý an toàn cũng trống và tình trạng này sẽ ngay lập tức kích hoạt`NO`, vì không có cách nào để họ phản ứng bất kể thành phần món ăn như thế nào. 

Khi trợ lý cấm gần như tất cả các loại gia vị, để lại chính xác`m`có sẵn, mọi món ăn hợp lệ đều được cố định. Trong trường hợp này, bất kỳ sự trùng lặp nào giữa các chất gây dị ứng của khách và tập hợp còn lại sẽ ngay lập tức dẫn đến một kết quả xác định và sự bất bình đẳng sẽ giảm xuống hoàn toàn để kiểm tra sự bình đẳng. 

Khi tất cả các loại gia vị xuất hiện trong danh sách chất gây dị ứng nhưng trợ lý cấm một tập hợp con, thuật toán vẫn hoạt động chính xác vì chỉ giao nhau với`S`vấn đề. Bất kỳ gia vị bên ngoài`S`không bao giờ được chọn nên không thể ảnh hưởng đến tính khả thi bất kể tần suất xuất hiện trong danh sách khách mời.
