---
title: "CF 102191E - Động tác rắn"
description: "Chuỗi di chuyển mô tả một bước đi trên lưới số nguyên vô hạn, bắt đầu từ một số ô ban đầu. Mỗi ký tự thay đổi ô hiện tại một đơn vị theo một trong bốn hướng chính."
date: "2026-08-18T02:40:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "E"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 614
verified: false
draft: false
---

[CF 102191E - Động tác của rắn](https://codeforces.com/problemset/problem/102191/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chuỗi di chuyển mô tả một bước đi trên lưới số nguyên vô hạn, bắt đầu từ một số ô ban đầu. Mỗi ký tự thay đổi ô hiện tại một đơn vị theo một trong bốn hướng chính. Chúng ta cần phần liền kề dài nhất của chuỗi di chuyển mà bước đi tương ứng của nó không bao giờ chiếm cùng một ô hai lần. 

Ô bắt đầu của chuỗi con đã chọn được tính là ô đã truy cập. Chi tiết này quan trọng vì một chuỗi con như`RL`đầu tiên di chuyển sang phải và sau đó ngay lập tức quay trở lại ô bắt đầu, vì vậy nó không hợp lệ. 

Một cách hữu ích để thể hiện bước đi là sử dụng các vị trí tiền tố. Gọi P i ​ là ô đến được sau lần tôi di chuyển đầu tiên, với P 0 ​ là ô bắt đầu. Một chuỗi con từ bước l đến bước r sẽ thăm chính xác các vị trí 

P l−1 ​ ,P l ​ ,…,P r ​ . 

Do đó, chuỗi con hợp lệ chính xác khi các vị trí tiền tố này đều khác biệt. Bài toán di chuyển ban đầu giờ đây đã trở thành bài toán mảng quen thuộc: tìm phạm vi vị trí tiền tố liền kề dài nhất không chứa giá trị trùng lặp. 

Độ dài có thể đạt tới 10 6. Thuật toán O(n 2 ) sẽ yêu cầu khoảng 5⋅10 11 lần lặp trong trường hợp xấu nhất, vượt xa giới hạn thời gian 1,5 giây cho phép. Chúng ta cần một phương pháp thời gian tuyến tính hoặc gần thời gian tuyến tính. Giới hạn bộ nhớ 256 MB cũng có nghĩa là việc lưu trữ một tập hợp lớn các đối tượng Python một cách bất cẩn có thể trở nên phù hợp, do đó việc triển khai nên sử dụng các biểu diễn tọa độ nhỏ gọn thay vì lưu trữ các bộ dữ liệu tọa độ. 

Có một số trường hợp đặc biệt dễ gây ra câu trả lời sai. Vì`R`, các ô được truy cập duy nhất là ô bắt đầu và ô ngay bên phải ô đó, vì vậy câu trả lời là`1`.```
1R
```Đầu ra đúng là`1`. Việc triển khai bất cẩn đếm các ô đã truy cập thay vì di chuyển có thể tạo ra kết quả`2`, trong khi câu trả lời bắt buộc là số lần di chuyển. 

Vì`RL`, con rắn quay trở lại ô ban đầu của nó.```
2RL
```Đầu ra đúng là`1`. Một phương pháp chỉ kiểm tra xem các bước di chuyển liên tiếp có khác nhau hay không sẽ chấp nhận toàn bộ chuỗi một cách sai lầm. Ô lặp lại có thể được phân tách bằng một số bước di chuyển, vì vậy chúng ta phải theo dõi vị trí thay vì chỉ hướng. 

Vì`RULD`, bước di chuyển cuối cùng sẽ quay trở lại ô ban đầu.```
4RULD
```Đầu ra đúng là`3`. Bước đi hoàn chỉnh bao gồm vị trí bắt đầu hai lần, nhưng ba bước đi đầu tiên tạo thành một bước đi hợp lệ. Thuật toán phải thu nhỏ cửa sổ hiện tại thay vì loại bỏ mọi thứ nhìn thấy trước đó. 

Một trường hợp tinh vi khác là vị trí mà lần xuất hiện trước đó nằm ngoài cửa sổ hiện tại. Ví dụ, hãy xem xét`RRLR`. Các vị trí tiền tố là 0,1,2,1,2. Khi thứ hai`1`xuất hiện, cửa sổ hợp lệ phải di chuyển qua cửa sổ cũ`1`. Sau này, khi`2`lặp lại, chỉ có cửa sổ hiện tại quan trọng. Một lỗi cửa sổ trượt phổ biến là gán trực tiếp ranh giới bên trái cho`last[position] + 1`, có thể di chuyển ranh giới về phía sau. Ranh giới chỉ có thể tiến về phía trước. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chọn mọi vị trí bắt đầu có thể và mở rộng chuỗi con từng bước một. Trong khi mở rộng, chúng tôi duy trì tọa độ hiện tại và một tập hợp các ô đã được truy cập. Nếu ô tiếp theo đã có trong tập hợp thì chuỗi con không thể được mở rộng nữa. Nếu không, chúng tôi chèn nó và cập nhật câu trả lời. Điều này đúng vì với mỗi vị trí bắt đầu, chúng tôi tìm thấy rõ ràng tiền tố hợp lệ dài nhất của hậu tố bắt đầu từ đó. 

Vấn đề là số lượng công việc lặp đi lặp lại. Trên một đầu vào như`RRRR...R`, mọi vị trí bắt đầu đều tạo ra một chuỗi con hợp lệ cho đến cuối. Thuật toán kiểm tra 

n+(n−1)+⋯+1= 2 n(n+1) ​ 

phần mở rộng, là 500000500000 cho n=10 6. Con số đó đã quá lớn, ngay cả trước khi tính đến các hoạt động đã định. 

Brute-force hoạt động vì nó duy trì độc lập tập hợp các ô cho từng điểm bắt đầu, nhưng nó sẽ loại bỏ thông tin bất cứ khi nào điểm bắt đầu thay đổi. Quan sát quan trọng là tất cả các chuỗi con tương ứng với các phạm vi liền kề của cùng một mảng vị trí tiền tố. Nếu chúng ta duy trì một cửa sổ các vị trí tiền tố không trùng lặp thì khi xuất hiện một bản sao, chúng ta chỉ cần di chuyển ranh giới bên trái đủ xa để loại bỏ sự xuất hiện trước đó của vị trí đó. 

Giả sử vị trí tiền tố hiện tại P i ​ được nhìn thấy lần cuối ở chỉ mục j. Nếu cửa sổ hợp lệ hiện tại bắt đầu ở L thì sự trùng lặp sẽ xảy ra chính xác khi j ≥L. Cửa sổ mới phải bắt đầu sau j, vì vậy chúng ta đặt 

L=max(L,j+1). 

Nếu j<L, lần xuất hiện cũ đã nằm ngoài cửa sổ hiện tại và không cần thay đổi gì. Mỗi vị trí tiền tố được xử lý một lần và ranh giới bên trái chỉ di chuyển về phía trước, đưa ra giải pháp O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n 2 ) | O(n) | Quá chậm | 
| Tối ưu | O(n) dự kiến ​​| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu thị ô bắt đầu ở vị trí tiền tố P 0 ​ =(0,0). Đối với mỗi lần di chuyển, hãy cập nhật tọa độ hiện tại để có được vị trí tiền tố tiếp theo. 
2. Duy trì một cửa sổ trượt của các chỉ số tiền tố [L,i] có các vị trí khác nhau. Chuỗi con nước đi tương ứng có độ dài i−L, vì nó bao gồm các nước đi L+1,…,i. 
3. Lưu trữ chỉ mục tiền tố gần đây nhất mà mọi tọa độ đã được truy cập. Khi P i ​ được tạo, hãy tra cứu chỉ số j trước đó của nó. 
4. Nếu j ≥L, vị trí hiện tại xảy ra bên trong cửa sổ, do đó cửa sổ không còn hợp lệ. Di chuyển ranh giới bên trái của nó tới j+1. Chúng tôi sử dụng`max`bởi vì ranh giới bên trái không bao giờ được phép lùi lại. 
5. Ghi lại`i`là lần xuất hiện mới nhất của vị trí hiện tại. Điều này phải xảy ra sau khi xác định ranh giới mới, vì những lần xuất hiện trong tương lai cần biết chỉ số hiện tại. 
6. Cửa sổ hiện tại chứa`i-L+1`vị trí tiền tố riêng biệt, tương ứng với`i-L`di chuyển. Cập nhật câu trả lời với`i-L`. 

Tại sao nó hoạt động: ở mỗi lần lặp, điều bất biến là tất cả các vị trí tiền tố từ P L ​ đến P i ​ đều khác biệt. Nếu vị trí mới không xuất hiện trước đó trong cửa sổ này thì bất biến vẫn đúng. Nếu nó xuất hiện tại j, mọi cửa sổ hợp lệ kết thúc tại i phải bắt đầu sau j, vì vậy việc chuyển L sang j+1 là cần thiết và đủ. Vì mọi chuỗi con hợp lệ kết thúc tại i phải được biểu thị bằng một cửa sổ không trùng lặp kết thúc tại i, nên cửa sổ lớn nhất như vậy sẽ đưa ra câu trả lời tốt nhất cho điểm cuối đó. Do đó, lấy mức tối đa trên tất cả các điểm cuối sẽ mang lại chuỗi con hợp lệ dài nhất trên toàn cầu. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve(s):    n = len(s)
    # Encode (x, y) into one integer so that the dictionary    # stores only integer keys instead of coordinate tuples.    base = 2 * n + 3    offset = n + 1
    x = 0    y = 0
    # last[position] = latest prefix index containing this position.    last = {offset * base + offset: 0}
    left = 0    ans = 0
    for i, ch in enumerate(s, 1):        if ch == 'R':            x += 1        elif ch == 'L':            x -= 1        elif ch == 'U':            y += 1        else:            y -= 1
        key = (x + offset) * base + (y + offset)
        previous = last.get(key)        if previous is not None and previous >= left:            left = previous + 1
```các`solve`chức năng xử lý các bước di chuyển chính xác như trong hướng dẫn.`x`Và`y`lưu trữ vị trí tiền tố hiện tại, trong khi`i`là chỉ số tiền tố của nó. 

Tọa độ nằm giữa −n và n, vì chỉ có n nước đi. biểu hiện```
Pythonkey = (x + offset) * base + (y + offset)
```ánh xạ mọi cặp tọa độ có thể thành một số nguyên duy nhất. Sử dụng một số nguyên làm khóa từ điển sẽ tiết kiệm bộ nhớ hơn đáng kể so với việc sử dụng bộ hai phần tử cho mỗi ô được truy cập, điều này quan trọng khi n lớn bằng 10 6. 

Vị trí ban đầu phải được chèn bằng chỉ mục`0`. Nếu không có nó, đường dẫn quay trở lại ô bắt đầu sẽ không được phát hiện. Đây chính xác là lý do tại sao`RL`phải có câu trả lời`1`còn hơn là`2`. 

điều kiện`previous >= left`là một kiểm tra ranh giới quan trọng khác. Sự xuất hiện trước đó trước cửa sổ hiện tại không làm cho cửa sổ hiện tại không hợp lệ. Viết`left = previous + 1`vô điều kiện sẽ di chuyển cửa sổ về phía sau một cách không chính xác. 

Câu trả lời là`i - left`, không`i - left + 1`. Cửa sổ chứa các vị trí tiền tố từ L đến i, nhưng có ít lần di chuyển hơn các vị trí. Số nguyên Python không bị tràn nên không cần xử lý đặc biệt cho mã hóa tọa độ hoặc câu trả lời. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`RULD`. Các vị trí tiền tố được mã hóa về mặt khái niệm dưới dạng tọa độ. 

| Di chuyển chỉ mục`i`| Di chuyển | Chức vụ`(x, y)`| Chỉ mục trước |`left`sau khi cập nhật | chiều dài hiện tại`i-left`|`ans`| 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | bắt đầu |`(0, 0)`| 0 | 0 | 0 | 0 | 
| 1 |`R`|`(1, 0)`| không | 0 | 1 | 1 | 
| 2 |`U`|`(1, 1)`| không | 0 | 2 | 2 | 
| 3 |`L`|`(0, 1)`| không | 0 | 3 | 3 | 
| 4 |`D`|`(0, 0)`| 0 | 1 | 3 | 3 | 

Ở nước đi thứ tư,`(0, 0)`trước đây đã được truy cập tại chỉ mục tiền tố`0`. Do đó, cửa sổ di chuyển từ các chỉ mục tiền tố`[0,4]`ĐẾN`[1,4]`. Bốn vị trí tiền tố đó chỉ tương ứng với ba nước đi, đưa ra đáp án đúng`3`. 

Đối với mẫu 2,`RRDDLLUUURDDR`, cơ chế tương tự giữ cửa sổ vị trí tiền tố không trùng lặp dài nhất. 

| Di chuyển chỉ mục`i`| Di chuyển | Chức vụ`(x, y)`| Chỉ mục trước |`left`| Độ dài hiện tại |`ans`| 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 |`R`|`(1,0)`| không | 0 | 1 | 1 | 
| 2 |`R`|`(2,0)`| không | 0 | 2 | 2 | 
| 3 |`D`|`(2,-1)`| không | 0 | 3 | 3 | 
| 4 |`D`|`(2,-2)`| không | 0 | 4 | 4 | 
| 5 |`L`|`(1,-2)`| không | 0 | 5 | 5 | 
| 6 |`L`|`(0,-2)`| không | 0 | 6 | 6 | 
| 7 |`U`|`(0,-1)`| không | 0 | 7 | 7 | 
| 8 |`U`|`(0,0)`| 0 | 1 | 7 | 7 | 
| 9 |`U`|`(0,1)`| không | 1 | 8 | 8 | 
| 10 |`R`|`(1,1)`| không | 1 | 9 | 9 | 
| 11 |`D`|`(1,0)`| 1 | 2 | 9 | 9 | 
| 12 |`D`|`(1,-1)`| không | 2 | 10 | 10 | 
| 13 |`R`|`(2,-1)`| 3 | 4 | 9 | 10 | 

Cửa sổ tối đa chứa mười bước di chuyển sau khi xử lý vị trí`12`. Tại vị trí`13`, lực tế bào lặp đi lặp lại`left`chuyển tiếp, vì vậy câu trả lời vẫn còn`10`. Bản sao cuối cùng không làm mất hiệu lực cửa sổ tốt nhất được phát hiện trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) dự kiến ​​| Mỗi lần di chuyển thực hiện một số thao tác từ điển và cập nhật tọa độ không đổi. | 
| Không gian | O(n) | Tối đa n+1 vị trí tiền tố riêng biệt được lưu trữ. | 

Với n<10 6, thuật toán chỉ thực hiện một lần chuyển qua chuỗi di chuyển. Từ điển có thể chứa nhiều nhất một mục nhập cho mỗi ô lưới riêng biệt được truy cập bởi bước đi, vì vậy kích thước của nó là tuyến tính tính bằng n. Mã hóa số nguyên tránh được chi phí lớn hơn nhiều khi lưu trữ các bộ dữ liệu tọa độ, giữ cho việc triển khai Python phù hợp với giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io

def solve(s):    n = len(s)    base = 2 * n + 3    offset = n + 1
    x = 0    y = 0
    last = {offset * base + offset: 0}
    left = 0    ans = 0
    for i, ch in enumerate(s, 1):        if ch == 'R':            x += 1        elif ch == 'L':            x -= 1        elif ch == 'U':            y += 1        else:            y -= 1
        key = (x + offset) * base + (y + offset)
        previous = last.get(key)        if previous is not None and previous >= left:            left = previous + 1
        last[key] = i        ans = max(ans, i - left)
    return ans

def run(inp: str) -> str:    data = inp.strip().split()    n = int(data[0])    s = data[1]    assert n == len(s)    return str(solve(s))

# Provided samplesassert run("4\nRULD\n") == "3", "sample 1"assert run("13\nRRDDLLUUURDDR\n") == "10", "sample 2"assert run("3\nRRU\n") == "3", "sample 3"
# Minimum-size inputassert run("1\nR\n") == "1", "minimum size"
# Immediate return to the starting cellassert run("2\nRL\n") == "1", "return to start"
# Repeated cells that require the sliding window to moveassert run("4\nRRLR\n") == "2", "repeated position"
# Maximum-size input, all moves in one directionlarge = "R" * 1_000_000assert run(f"{len(large)}\n{large}\n") == "1000000", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / R`|`1`| Kích thước đầu vào tối thiểu và chuyển đổi từ vị trí tiền tố sang số lần di chuyển | 
|`2 / RL`|`1`| Quay lại ô ban đầu | 
|`4 / RRLR`|`2`| Chuyển động chính xác của ranh giới cửa sổ trượt | 
|`1000000 / R...R`|`1000000`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu`1 / R`, thuật toán bắt đầu với vị trí tiền tố`(0,0)`. Sau đó`R`, nó đạt tới`(1,0)`, điều chưa từng xuất hiện trước đây. Cửa sổ là`[0,1]`, chứa hai vị trí riêng biệt và do đó có một nước đi. Đầu ra là`1`. 

Vì`RL`, các vị trí tiền tố là`(0,0)`,`(1,0)`, Và`(0,0)`. Khi vị trí cuối cùng được tạo ra, chỉ số trước đó của nó là`0`, nằm bên trong cửa sổ hiện tại bắt đầu từ`0`. Thuật toán thay đổi`left`ĐẾN`1`, để lại vị trí tiền tố`1`Và`2`. Điều đó thể hiện một động thái hợp lệ duy nhất`L`, vì vậy đầu ra là`1`. 

Vì`RULD`, bước thứ tư đạt tới`(0,0)`, lần xuất hiện trước đó là ở chỉ mục`0`. Ranh giới bên trái thay đổi từ`0`ĐẾN`1`, để lại chuỗi con ba bước hợp lệ`ULD`. Câu trả lời đã có rồi`3`, do đó ô bắt đầu lặp lại không làm cho thuật toán mất chuỗi con tốt nhất trước đó. 

Vì`RRLR`, các vị trí tiền tố là`(0,0)`,`(1,0)`,`(2,0)`,`(1,0)`,`(2,0)`. Tại chỉ mục`3`,`(1,0)`được nhìn thấy lần cuối tại chỉ mục`1`, Vì thế`left`trở thành`2`. Tại chỉ mục`4`,`(2,0)`được nhìn thấy lần cuối tại chỉ mục`2`, chính xác là ranh giới bên trái hiện tại, vì vậy`left`trở thành`3`. Cửa sổ lớn nhất có chiều dài`2`, tương ứng với hai nước đi đầu tiên`RR`, và đầu ra là`2`. Trường hợp này đặc biệt kiểm tra rằng`left`chỉ được nâng cao khi cần thiết. 

Đối với đầu vào có kích thước tối đa bao gồm một triệu`R`ký tự, mỗi vị trí tiền tố đều khác nhau vì tọa độ x tăng thêm một sau mỗi lần di chuyển. Không có bản sao nào được tìm thấy, vì vậy`left`còn lại`0`và câu trả lời tăng lên`1000000`. Thuật toán thực hiện một lần lặp cho mỗi ký tự, chứng minh tại sao cách tiếp cận O(n) xử lý giới hạn trên trong khi cách tiếp cận bậc hai không thể.
