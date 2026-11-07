---
title: "CF 1333B - Loại Anton"
description: "Chúng ta được cho hai mảng số nguyên có cùng độ dài. Mảng đầu tiên bắt đầu với các giá trị rất hạn chế, mỗi vị trí có thể là giá trị âm, số 0 hoặc giá trị dương. Mảng thứ hai có thể chứa các số nguyên tùy ý, có thể có độ lớn rất lớn."
date: "2026-06-16T08:38:58+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1333
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 632 (Div. 2)"
rating: 1100
weight: 1333
solve_time_s: 317
verified: false
draft: false
---

[CF 1333B - Kind Anton](https://codeforces.com/problemset/problem/1333/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 5 phút 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên có cùng độ dài. Mảng đầu tiên bắt đầu với các giá trị rất hạn chế, mỗi vị trí có thể là giá trị âm, số 0 hoặc giá trị dương. Mảng thứ hai có thể chứa các số nguyên tùy ý, có thể có độ lớn rất lớn. Nhiệm vụ là quyết định xem chúng ta có thể chuyển đổi mảng đầu tiên thành mảng thứ hai hay không bằng cách sử dụng một thao tác cụ thể liên tục sửa đổi các phần tử. 

Thao tác được phép chọn hai chỉ số có chỉ số đầu tiên đúng trước chỉ số thứ hai, sau đó thêm giá trị của vị trí trước đó vào vị trí sau. Điều quan trọng là vị trí nguồn không thay đổi và thao tác có thể được lặp lại bất kỳ số lần nào, kể cả việc sử dụng lại cùng một cặp. 

Hạn chế chính là ảnh hưởng chỉ chảy từ trái sang phải. Các yếu tố trước đó có thể ảnh hưởng đến những yếu tố sau, nhưng không bao giờ có tác động ngược lại. Điều này tạo ra một hệ thống truyền định hướng trên toàn mảng. 

Kích thước đầu vào cho phép tổng cộng lên tới một trăm nghìn phần tử trong tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi mô phỏng hoạt động bậc hai giữa các cặp chỉ số, vì ngay cả một nỗ lực ngây thơ duy nhất để mô phỏng các cập nhật lặp đi lặp lại cho mỗi vị trí cũng sẽ bùng nổ vượt xa giới hạn thời gian. 

Một khó khăn tinh vi xuất hiện trong việc hiểu hoạt động lặp đi lặp lại thực sự mang lại điều gì. Vì cùng một cặp có thể được chọn nhiều lần nên một giá trị khác 0 có thể được áp dụng lặp đi lặp lại cho các vị trí sau này, điều đó có nghĩa là thao tác này không phải là thao tác chuyển một lần mà là nguồn ảnh hưởng có thể tái sử dụng. 

Các trường hợp cạnh có xu hướng phá vỡ lý luận ngây thơ bao gồm các tình huống trong đó mức tăng yêu cầu xuất hiện sớm nhưng nguồn tích cực duy nhất xảy ra sau đó, ví dụ như một mảng như`[0, 1]`cố gắng để trở thành`[1, 0]`. Điều này là không thể vì giá trị dương ở quá xa bên phải để giúp ích cho vị trí đầu tiên. Một trường hợp thất bại khác là giả định tổng số tiền là quan trọng. Ví dụ, việc khớp tổng số tiền không đảm bảo tính khả thi vì các hoạt động mang tính định hướng và không cho phép sắp xếp lại khối lượng ngược. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ mô phỏng trực tiếp các hoạt động. Chúng tôi sẽ liên tục quét tất cả các cặp`(i, j)`và áp dụng các bản cập nhật bất cứ khi nào nó có vẻ hữu ích cho đến khi không có thay đổi nào xảy ra nữa hoặc mảng khớp với mục tiêu. Cách tiếp cận này đúng về nguyên tắc vì nó mô phỏng chính xác các thao tác được phép, nhưng mỗi thao tác có thể được áp dụng vô thời hạn và có các cặp O(n^2), dẫn đến số lần lặp không giới hạn hoặc cực kỳ lớn. Ngay cả việc giới hạn bản thân ở mức giới hạn hợp lý vẫn sẽ không thể mở rộng đến n lên tới 100000. 

Quan sát quan trọng là cấu trúc của hoạt động cực kỳ thô. Mỗi vị trí trong mảng ban đầu là nguồn ảnh hưởng tích cực, ảnh hưởng tiêu cực hoặc trung lập vĩnh viễn. Giá trị của một có nghĩa là chúng ta có thể liên tục thêm một vào bất kỳ vị trí nào sau này. Giá trị trừ một có nghĩa là chúng ta có thể trừ đi nhiều lần một từ bất kỳ vị trí nào sau đó. Số 0 không làm gì cả. 

Điều này biến toàn bộ quá trình thành một điều kiện có thể tiếp cận hơn là một vấn đề mô phỏng. Đối với mỗi vị trí trong mảng mục tiêu, chúng ta chỉ cần biết liệu chúng ta đã gặp ít nhất một nguồn dương và ít nhất một nguồn âm trong tiền tố cho đến vị trí đó hay chưa. Nếu chúng tôi cần tăng giá trị so với trạng thái ban đầu của nó, chúng tôi yêu cầu quyền truy cập vào nguồn dương ở tiền tố bên trái. Nếu cần giảm nó, chúng tôi yêu cầu nguồn âm ở tiền tố bên trái. Khi một nguồn như vậy tồn tại, nó có thể được tái sử dụng vô số lần, vì vậy chúng ta không bao giờ cần nhiều hơn một nguồn. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Theo dõi nguồn tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập, quét từ trái sang phải trong khi theo dõi xem chúng tôi đã thấy trường hợp dương tính hay âm tính cho đến nay trong mảng ban đầu. 

1. Khởi tạo hai cờ boolean, một cờ cho biết cờ dương có xuất hiện trong tiền tố hay không và cờ còn lại cho biết cờ âm có xuất hiện hay không. 
2. Lặp lại từng chỉ mục từ trái sang phải. Tại mỗi vị trí, trước tiên hãy cập nhật các cờ bằng giá trị từ mảng ban đầu. Nếu phần tử hiện tại là một, cờ dương sẽ trở thành đúng. Nếu nó trừ một, cờ âm sẽ trở thành đúng. Số không không thay đổi bất cứ điều gì. 
3. So sánh giá trị hiện tại của hai mảng tại vị trí này. Nếu giá trị đích lớn hơn giá trị ban đầu thì chúng ta cần tăng vị trí này bằng cách thêm một giá trị, điều này yêu cầu nguồn dương đã tồn tại trong tiền tố. Nếu không có nguồn như vậy tồn tại thì việc chuyển đổi là không thể. 
4. Nếu giá trị đích nhỏ hơn giá trị ban đầu thì chúng ta cần giảm giá trị đó bằng cách sử dụng thao tác trừ một, yêu cầu phải tồn tại nguồn âm trong tiền tố. Nếu không tồn tại, việc chuyển đổi là không thể. 
5. Nếu không có điều kiện nào bị vi phạm đối với bất kỳ chỉ mục nào thì việc chuyển đổi có thể thực hiện được. 

Điểm quan trọng là chúng ta không bao giờ cần theo dõi số lần một nguồn được sử dụng, mà chỉ cần theo dõi xem nguồn đó có tồn tại trong tiền tố có thể truy cập hay không. 

### Tại sao nó hoạt động 

Cấu trúc hoạt động làm cho mỗi mục nhập khác 0 trong mảng ban đầu trở thành một trình tạo có thể tái sử dụng các mức tăng hoặc giảm đơn vị cho tất cả các vị trí sau này. Khi giá trị của một xuất hiện ở chỉ số i nào đó, mọi vị trí j lớn hơn i có thể nhận được nhiều mức tăng tùy ý từ i bằng cách lặp lại thao tác. Điều tương tự cũng xảy ra với trường hợp trừ một là nguồn giảm không giới hạn. Bởi vì các nguồn không bao giờ được sử dụng mà chỉ được đưa vào tiền tố nên trạng thái có ý nghĩa duy nhất là liệu có ít nhất một nguồn tồn tại ở mỗi bên hay không. 

Điều này làm giảm tất cả các câu hỏi về tính khả thi đối với việc kiểm tra sự tồn tại của tiền tố, vốn vẫn nhất quán trong suốt quá trình quét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        has_pos = False
        has_neg = False
        ok = True
        
        for i in range(n):
            if a[i] == 1:
                has_pos = True
            elif a[i] == -1:
                has_neg = True
            
            if b[i] > a[i]:
                if not has_pos:
                    ok = False
                    break
            elif b[i] < a[i]:
                if not has_neg:
                    ok = False
                    break
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo quá trình quét được mô tả trong thuật toán. Các cờ được cập nhật trước khi kiểm tra tính khả thi ở mỗi chỉ mục vì bản thân vị trí hiện tại có thể đóng vai trò là nguồn cho các chỉ mục sau này nếu nó khác 0. 

Một lỗi tinh vi phổ biến là cập nhật cờ sau khi kiểm tra điều kiện. Điều đó sẽ ngăn không cho một giá trị ở vị trí i được sử dụng để đáp ứng các ràng buộc tại chính vị trí i khi được phép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1, -1, 0]
b = [1, 1, -2]
```Chúng tôi theo dõi các nguồn tiền tố và tính khả thi từng bước. 

| tôi | một [tôi] | b[i] | has_pos | has_neg | Tình trạng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | Đúng | Sai | được | 
| 1 | -1 | 1 | Đúng | Đúng | tăng cho phép | 
| 2 | 0 | -2 | Đúng | Đúng | cho phép giảm | 

Ở chỉ số 1, đích lớn hơn giá trị ban đầu nên chúng ta cần một nguồn dương, vốn đã tồn tại từ chỉ số 0. Ở chỉ số 2, đích nhỏ hơn nên chúng ta cần một nguồn âm, xuất hiện ở chỉ số 1. Mọi yêu cầu đều được thỏa mãn. 

### Ví dụ 2 

đầu vào:```
n = 2
a = [1, 0]
b = [1, 41]
```| tôi | một [tôi] | b[i] | has_pos | has_neg | Tình trạng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | Đúng | Sai | được | 
| 1 | 0 | 41 | Đúng | Sai | tăng cho phép | 

Mặc dù mức tăng lớn nhưng sự hiện diện của một nguồn tích cực duy nhất là đủ vì nó có thể được áp dụng nhiều lần cho chỉ số thứ hai. 

Điều này chứng tỏ rằng độ lớn không thành vấn đề, chỉ có sự tồn tại của một nguồn mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | mỗi phần tử được xử lý một lần với công việc liên tục | 
| Không gian | O(1) thêm | chỉ có hai cờ boolean được duy trì | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, dễ dàng phù hợp với giới hạn lên tới một trăm nghìn phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        has_pos = False
        has_neg = False
        ok = True
        
        for i in range(n):
            if a[i] == 1:
                has_pos = True
            elif a[i] == -1:
                has_neg = True
            
            if b[i] > a[i] and not has_pos:
                ok = False
                break
            if b[i] < a[i] and not has_neg:
                ok = False
                break
        
        print("YES" if ok else "NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided samples
assert run("""5
3
1 -1 0
1 1 -2
3
0 1 1
0 2 2
2
1 0
1 41
2
-1 0
-1 -41
5
0 1 -1 1 -1
1 1 -1 1 -1
""") == """YES
NO
YES
YES
NO"""

# custom cases
assert run("""1
1
0
5
""") == "NO", "no source cannot increase"

assert run("""1
3
1 0 0
1 100 100
""") == "YES", "single positive enables unlimited increases"

assert run("""1
3
0 -1 0
0 -5 -5
""") == "YES", "negative source enables decreases"

assert run("""1
3
0 1 0
-1 1 0
""") == "NO", "negative needed before it appears"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tăng mục tiêu duy nhất bằng 0 | KHÔNG | không có nguồn | 
| nguồn dương đơn tăng lớn | CÓ | tái sử dụng không giới hạn +1 | 
| lan truyền tiêu cực | CÓ | -1 cho phép giảm tùy ý | 
| đặt hàng phụ thuộc thất bại | KHÔNG | thực thi ràng buộc tiền tố | 

## Vỏ cạnh 

Trường hợp cạnh phổ biến là khi một thao tác được yêu cầu phụ thuộc vào nguồn xuất hiện sau trong mảng. Ví dụ: nếu vị trí đầu tiên yêu cầu giảm nhưng điểm trừ đầu tiên chỉ xuất hiện sau vị trí đó, thì phép chuyển đổi không thành công ngay cả khi tồn tại âm toàn cục. Thuật toán từ chối điều này một cách chính xác vì cờ tiền tố vẫn sai tại thời điểm đánh giá. 

Một trường hợp tinh vi khác là khi tất cả các phần tử của mảng ban đầu đều bằng 0. Vì các số 0 không thể tạo ra bất kỳ thay đổi nào nên bất kỳ mảng mục tiêu nào khác với mảng mục tiêu ban đầu sẽ ngay lập tức bị lỗi, điều này được ghi lại một cách chính xác do không có cả hai cờ trong suốt quá trình quét. 

Trường hợp thứ ba là khi chỉ tồn tại một loại nguồn. Nếu mảng chỉ chứa một, thì mọi mức giảm bắt buộc đều không thể thực hiện được bất kể cấu trúc trong tương lai như thế nào và quá trình quét sẽ ngay lập tức phát hiện điều này khi một yêu cầu phủ định xuất hiện mà không có tiền tố phủ định.
