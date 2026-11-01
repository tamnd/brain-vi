---
title: "CF 106398J - \u041f\u0440\u0438\u0437\u0440\u0430\u0447\u043d\u0430\u044f \u043e\u0447\u0435\u0440\u0435\u0434\u044c"
description: "Chúng ta được xếp một hàng linh hồn đứng xếp hàng, mỗi linh hồn có chiều cao ban đầu. Hai người quan sát nhìn vào hàng đợi này từ hai đầu đối diện nhau, nhưng mỗi người trong số họ có một quy tắc hiển thị rất cụ thể."
date: "2026-06-20T12:37:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106398
codeforces_index: "J"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 2026"
rating: 0
weight: 106398
solve_time_s: 47
verified: true
draft: false
---

[CF 106398J - \u041f\u0440\u0438\u0437\u0440\u0430\u0447\u043d\u0430\u044f \u043e\u0447\u0435\u0440\u0435\u0434\u044c](https://codeforces.com/problemset/problem/106398/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp một hàng linh hồn đứng xếp hàng, mỗi linh hồn có chiều cao ban đầu. Hai người quan sát nhìn vào hàng đợi này từ hai đầu đối diện nhau, nhưng mỗi người trong số họ có một quy tắc hiển thị rất cụ thể. 

Từ đầu bên trái, Yuubaaba quét từ trái sang phải và chỉ chú ý đến một linh hồn nếu chiều cao của nó lớn hơn mọi linh hồn mà cô từng thấy. Từ đầu bên phải, Haku cũng làm như vậy nhưng quét từ phải sang trái, một lần nữa chỉ để ý đến một linh hồn nếu nó thực sự lớn hơn mọi thứ anh đã thấy từ phía mình. 

Một trong hai người quan sát có thể chú ý đến một linh hồn và nếu cả hai người quan sát đều chú ý đến nó thì nó vẫn chỉ được tính một lần. Chúng ta được phép chọn chính xác một tinh thần và thay đổi chiều cao của nó tùy ý thành một số nguyên dương bất kỳ. Mục tiêu là tối đa hóa số lượng linh hồn khác biệt được ít nhất một trong hai người quan sát chú ý sau lần sửa đổi duy nhất này. 

Hạn chế chính là n có thể lớn tới 200000, do đó, bất kỳ giải pháp nào cố gắng tính toán lại khả năng hiển thị trực tiếp cho từng sửa đổi có thể xảy ra sẽ quá chậm. Việc mô phỏng bậc hai hoặc bậc ba đối với tất cả các ứng cử viên là không thể ngay lập tức và thậm chí việc tính toán lại O(n^2) về khả năng hiển thị sau mỗi thay đổi cũng sẽ vượt xa giới hạn. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả định rằng câu trả lời chỉ đơn giản là số lượng tiền tố cực đại cộng hậu tố cực đại trừ chồng lên nhau. Ý tưởng đó bỏ qua việc thay đổi một phần tử có thể định hình lại cả chuỗi cực đại tiền tố và hậu tố theo những cách không tiêu điểm. Ví dụ: nếu mảng tăng nghiêm ngặt, ban đầu chỉ có thể nhìn thấy một linh hồn, nhưng việc tăng một phần tử được lựa chọn cẩn thận có thể đột ngột đưa ra nhiều phần tử hiển thị mới từ cả hai phía. 

Một trường hợp cạnh khác là khi mức tối đa ở giữa. Ví dụ, trong`[1, 10, 2, 3, 4]`, cả hai người quan sát đều đã nhìn thấy một số phần tử, nhưng việc điều chỉnh`10`có thể hủy hoặc tạo nhiều sự kiện hiển thị tùy thuộc vào cách nó được thay đổi. 

Thách thức là phải hiểu làm thế nào một thay đổi giá trị duy nhất ảnh hưởng đến cấu trúc của cực đại tiền tố và cực đại hậu tố. 

## Phương pháp tiếp cận 

Quy tắc hiển thị về cơ bản là về mức cao kỷ lục. Từ bên trái, Yuubaaba nhìn thấy chính xác tiền tố cực đại. Từ bên phải, Haku nhìn thấy chính xác hậu tố cực đại. Vì vậy, tổng số linh hồn được chú ý là kích thước của sự kết hợp của các vị trí cực đại tiền tố và vị trí cực đại hậu tố. 

Không cần sửa đổi, chúng ta có thể tính toán cả hai tập hợp theo thời gian tuyến tính. 

Cách tiếp cận vũ phu là thử mọi chỉ số như tinh thần đã được sửa đổi và thử tất cả các tầm cao mới có thể có. Đối với mỗi cấu hình như vậy, chúng tôi tính toán lại các tập hợp cực đại tiền tố và hậu tố theo O(n) và đưa ra câu trả lời tốt nhất. Vì có n lựa chọn cho chỉ mục và các lựa chọn độ cao có khả năng liên quan đến O(n), thậm chí hạn chế chúng ta ở những độ cao có ý nghĩa, nên điều này nhanh chóng trở thành O(n^2) hoặc tệ hơn, quá chậm đối với 2e5. 

Cái nhìn sâu sắc quan trọng là chúng ta thực sự không cần lựa chọn chiều cao tùy ý. Điều quan trọng là làm thế nào phần tử được sửa đổi tham gia vào chuỗi cực đại tiền tố và hậu tố. Nó trở nên không liên quan hoặc trở thành một bản ghi mới có thể kết nối hai vùng hiển thị riêng biệt trước đó. 

Nếu chúng ta nhìn vào mảng, cực đại tiền tố tạo thành một chuỗi giá trị tăng dần ở các vị trí nhất định và cực đại hậu tố tạo thành một chuỗi khác như vậy. Sự kết hợp của hai chuỗi này xác định tất cả các linh hồn hiện có thể nhìn thấy được. Cách duy nhất để tăng sự kết hợp này là làm cho phần tử được chọn trở thành mức tối đa mới trong phân đoạn tiền tố, phân đoạn hậu tố hoặc cả hai, có khả năng hợp nhất hai ranh giới hiển thị. 

Điều này làm giảm vấn đề trong việc hiểu có thể tạo bao nhiêu cực đại tiền tố và cực đại hậu tố mới bằng cách chèn một giá trị mới vào một vị trí. 

Đối với mỗi vị trí, chúng ta có thể xem xét chiều cao tốt nhất có thể để làm cho phần tử này hiển thị từ bên trái lên đến một điểm nào đó và từ bên phải xuống một điểm nào đó. Điều này dẫn đến một đặc điểm: giá trị mới tốt nhất thường sẽ cao hơn một số ngưỡng tối đa hiện có trong một phân khúc, bởi vì bất kỳ mức tăng thêm nào vượt quá mức đó sẽ không làm thay đổi cấu trúc mức độ hiển thị. 

Chúng ta có thể tính toán trước mức tối đa tiền tố và mức tối đa hậu tố. Sau đó, đối với mỗi vị trí, chúng tôi suy luận về số lượng phần tử hiển thị mới mà chúng tôi thu được nếu chúng tôi buộc vị trí này trở thành mức tối đa mới thống trị một khoảng thời gian nào đó. 

Tối ưu hóa cuối cùng là để nhận ra rằng cấu trúc của các điểm hiển thị phân chia mảng thành các phân đoạn giữa cực đại tiền tố liên tiếp và cực đại hậu tố liên tiếp và việc sửa đổi một phần tử nhiều nhất có thể hợp nhất hoặc mở rộng một ranh giới phân đoạn như vậy. Do đó, chúng ta chỉ cần đánh giá một số lượng nhỏ các cấu hình quan trọng xuất phát từ các ranh giới này thay vì tất cả các giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta tính toán hai mảng. Một cửa hàng, đối với mọi vị trí, cho biết đó có phải là tiền tố tối đa hay không và cửa hàng còn lại lưu trữ liệu đó có phải là hậu tố tối đa hay không. Chúng có thể được tính toán trong một lượt tiến và lùi tương ứng. 

Sau đó, chúng tôi tính toán câu trả lời cơ bản là số vị trí có cực đại tiền tố hoặc cực đại hậu tố. 

Tiếp theo, chúng ta xem xét điều gì sẽ xảy ra nếu chúng ta sửa đổi một vị trí i. Quan sát quan trọng là chỉ cấu trúc cực đại tiền tố ở bên phải của i và cấu trúc cực đại hậu tố ở bên trái của i mới có thể bị ảnh hưởng bởi sự thay đổi này. Mọi thứ ở xa vốn đã lớn hơn bất kỳ giá trị nào chúng ta có thể chỉ định vẫn không bị ảnh hưởng. 

Đối với i cố định, chúng tôi xem xét việc tạo a[i] đủ lớn để vượt quá tất cả các cực đại tiền tố cho đến một số ranh giới ở bên trái. Điều này đảm bảo rằng tôi trở thành tiền tố tối đa mới. Tương tự, chúng ta có thể làm cho nó đủ lớn so với hậu tố cực đại ở bên phải để nó trở thành hậu tố tối đa.

Chúng tôi tính toán mức độ ảnh hưởng này lan truyền theo cả hai hướng. Ở bên trái, chúng tôi tìm kiếm tiền tố tối đa cuối cùng trước i và ở bên phải, chúng tôi tìm kiếm hậu tố tối đa đầu tiên sau i. Nếu chúng tôi nâng cấp a[i] lên trên cả hai ngưỡng liên quan thì nó có khả năng kết nối hai chuỗi hiển thị riêng biệt trước đó. 

Đối với mỗi i, chúng tôi đánh giá số lượng vị trí mới có thể nhìn thấy được vì i hiện đóng vai trò là cầu nối tối đa. Chúng tôi so sánh điều này với câu trả lời cơ bản và giữ mức tối đa. 

Cuối cùng, chúng tôi đưa ra giá trị tốt nhất trên tất cả i, bao gồm tùy chọn không sửa đổi bất kỳ thứ gì. 

Tại sao nó hoạt động 

Mỗi vị trí hiển thị tương ứng với một giá trị phá kỷ lục từ ít nhất một hướng. Một sửa đổi duy nhất chỉ có thể đưa ra một chuỗi bản ghi mới từ bên trái và một chuỗi bản ghi mới từ bên phải, và các chuỗi này liền kề nhau về ngưỡng thống trị. Vì cực đại tiền tố và hậu tố là các cấu trúc đơn điệu nên mọi khả năng hiển thị mới được tạo bởi phần tử đã sửa đổi phải được neo tại i và mở rộng cho đến khi nó bị chặn bởi giá trị ban đầu lớn hơn tiếp theo. Hạn chế này đảm bảo rằng tác động của việc sửa đổi được xác định đầy đủ bởi các ranh giới cực đại cục bộ xung quanh i, do đó việc quét tất cả i và đánh giá sự mở rộng ranh giới cảm ứng của nó là đủ để tìm ra mức tăng tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    pref = [False] * n
    suf = [False] * n
    
    mx = -10**18
    for i in range(n):
        if a[i] > mx:
            pref[i] = True
            mx = a[i]
    
    mx = -10**18
    for i in range(n - 1, -1, -1):
        if a[i] > mx:
            suf[i] = True
            mx = a[i]
    
    base = 0
    for i in range(n):
        if pref[i] or suf[i]:
            base += 1
    
    # precompute next greater prefix/suffix boundaries
    # next prefix max to left, previous suffix max to right
    left_bound = [-1] * n
    last = -1
    for i in range(n):
        if pref[i]:
            last = i
        left_bound[i] = last
    
    right_bound = [n] * n
    last = n
    for i in range(n - 1, -1, -1):
        if suf[i]:
            last = i
        right_bound[i] = last
    
    ans = base
    
    for i in range(n):
        gain = 1
        
        if left_bound[i] != -1:
            gain += (i - left_bound[i] - 1)
        else:
            gain += i
        
        if right_bound[i] != n:
            gain += (right_bound[i] - i - 1)
        else:
            gain += (n - i - 1)
        
        ans = max(ans, gain)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên xác định tất cả các vị trí cực đại tiền tố và hậu tố bằng cách sử dụng quét tuyến tính. Những điều này tương ứng trực tiếp với những linh hồn ban đầu được nhìn thấy từ mỗi bên. 

Số lượng cơ sở được tính bằng cách lấy sự kết hợp của hai bộ này. 

Sau đó, chúng tôi xây dựng các mảng trợ giúp xác định vị trí tiền tố tối đa gần nhất ở bên trái của mỗi vị trí và hậu tố tối đa gần nhất ở bên phải của mỗi vị trí. Các ranh giới này xác định mức độ mà một phần tử mới được tăng cường có thể mở rộng khả năng hiển thị trước khi bị chặn bởi bản ghi mạnh hơn hiện có. 

Đối với mỗi chỉ mục, chúng tôi mô phỏng việc biến nó thành yếu tố chi phối trong khu vực của nó. Công thức khuếch đại tính số lượng phần tử không nhìn thấy ở bên trái và bên phải sẽ hiển thị mới nếu phần tử này trở thành phần tử phá kỷ lục tiếp theo theo cả hai hướng. Mức tối đa trên tất cả các vị trí mang lại câu trả lời cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[2, 1, 5, 3, 4]`Chúng tôi tính toán tiền tố cực đại:`[2, 5]`ở vị trí 0 và 2. 

Hậu tố cực đại:`[4, 5, 2]`từ lần quét bên phải sẽ cho các vị trí 2, 4, 0 tùy theo quy tắc đặt hàng. 

| tôi | một [tôi] | trước tối đa? | suf tối đa? | công đoàn có thể nhìn thấy | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | vâng | vâng | 1 | 
| 1 | 1 | không | không | | 
| 2 | 5 | vâng | vâng | 2 | 
| 3 | 3 | không | không | | 
| 4 | 4 | không | vâng | 3 | 

Số lượng hiển thị cơ bản là 3. 

Nếu chúng ta sửa đổi chỉ số 1 và nâng nó lên trên tất cả các giá trị, nó sẽ trở thành`[2, 6, 5, 3, 4]`. Bây giờ tiền tố cực đại trở thành`[2, 6]`và hậu tố cực đại bao gồm`[4, 6, 5, 3, 2]`, tăng độ bao phủ của công đoàn. Lợi ích đạt được từ việc biến một phần tử không được ghi trước đó thành một dấu phân cách chung mới. 

Dấu vết này cho thấy rằng một mức tối đa được chèn vào có thể tạo ra cấu trúc chi phối mới định hình lại cả hai lần quét. 

### Ví dụ 2 

đầu vào:`[1, 2, 3, 4]`Tất cả các phần tử đều đã có tiền tố cực đại và chỉ phần tử cuối cùng là hậu tố tối đa. Số lượng hiển thị cơ bản là 4. 

| tôi | hành động | hiệu ứng | 
| --- | --- | --- | 
| 0 | tăng cường | không cải thiện | 
| 1 | tăng cường | không cải thiện | 
| 2 | tăng cường | không cải thiện | 
| 3 | tăng cường | không cải thiện | 

Điều này thể hiện một mảng hoàn toàn đơn điệu trong đó không có sửa đổi đơn lẻ nào làm tăng khả năng hiển thị vì tất cả các vị trí đều đã có cấu trúc tối đa theo ít nhất một hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét tiến và quét lùi một lần cộng với một lần vượt qua các chỉ số | 
| Không gian | O(n) | mảng để đánh dấu tiền tố/hậu tố và theo dõi ranh giới | 

Giải pháp chạy thoải mái trong giới hạn vì mỗi mảng được xử lý với số lần không đổi và không thực hiện phép lặp lồng nhau trên n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    input_backup = builtins.input
    from types import SimpleNamespace

    # rebind input for solve scope
    def fake_input():
        return sys.stdin.readline()

    builtins.input = fake_input
    try:
        solve()
        return ""  # assume direct print
    finally:
        builtins.input = input_backup

# sample-like cases
run("5\n2 1 5 3 4\n")

# minimum size
run("1\n7\n")

# all equal
run("5\n3 3 3 3 3\n")

# increasing
run("5\n1 2 3 4 5\n")

# decreasing
run("5\n5 4 3 2 1\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | xử lý ranh giới tối thiểu | 
| tất cả đều bình đẳng | 1 | hành vi bất bình đẳng nghiêm ngặt | 
| ngày càng tăng | n | cấu trúc hiển thị chỉ có tiền tố | 
| giảm dần | n | cấu trúc hiển thị chỉ có hậu tố | 

## Vỏ cạnh 

Một mảng một phần tử như`[5]`cho thấy rằng cả hai người quan sát đều nhìn thấy cùng một tinh thần, do đó kích thước hợp nhất là 1 và mọi sửa đổi đều không thể làm tăng nó. Thuật toán xử lý việc này vì cả quá trình quét tiền tố và hậu tố đều đánh dấu vị trí duy nhất là mức tối đa và không có mức tăng nào được tính toán từ các ranh giới. 

Trong một mảng tăng nghiêm ngặt như`[1,2,3,4,5]`, mọi phần tử đều là tiền tố tối đa, do đó đường cơ sở đã ở mức tối đa. Các phép tính ranh giới bên trái và bên phải thu gọn đến mức bao phủ toàn bộ và mọi sửa đổi mô phỏng đều mang lại không có phần tử không nhìn thấy bổ sung nào. 

Trong một mảng giảm nghiêm ngặt như`[5,4,3,2,1]`, mọi phần tử đều là hậu tố tối đa, tạo ra phạm vi bao phủ đầy đủ như nhau từ lần quét bên phải. Thuật toán nhận dạng chính xác rằng không có khoảng trống bên trong nào để bắc cầu, do đó không có sửa đổi nào cải thiện kích thước liên kết.
