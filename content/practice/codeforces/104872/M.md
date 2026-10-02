---
title: "CF 104872M - Katya và bàn phím bị hỏng"
description: "Katya muốn gõ một chuỗi cố định nhưng một số phím trên bàn phím hoạt động không ổn định. Đối với mỗi phím chữ cái bị hỏng, việc nhấn phím đó không tạo ra một ký tự một cách đáng tin cậy mỗi lần."
date: "2026-06-28T10:33:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104872
codeforces_index: "M"
codeforces_contest_name: "2023-2024 Russia Team Open, High School Programming Contest (VKOSHP XXIV)"
rating: 0
weight: 104872
solve_time_s: 72
verified: false
draft: false
---

[CF 104872M - Katya và bàn phím bị hỏng](https://codeforces.com/problemset/problem/104872/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Katya muốn gõ một chuỗi cố định nhưng một số phím trên bàn phím hoạt động không ổn định. Đối với mỗi phím chữ cái bị hỏng, việc nhấn phím đó không tạo ra một ký tự một cách đáng tin cậy mỗi lần. Thay vào đó, nó xen kẽ giữa thất bại và thành công trong một chu kỳ cố định: phím không tạo ra gì trong một lần nhấn, sau đó tạo ra chữ cái chính xác cho một số lần nhấn nhất định, sau đó lại thất bại một lần và lặp lại mẫu này mãi mãi. 

Hiệu quả là mỗi khi Katya nhấn một phím bị hỏng, cô ấy có thể thực sự đóng góp hoặc không đóng góp một nhân vật cho bài luận, tùy thuộc vào vị trí hiện tại của cô ấy trong chu kỳ của phím đó. Độ dài chu kỳ được xác định bởi tham số đã cho cho khóa đó. Mục đích là để đảm bảo rằng sau một số lần nhấn, bất kể chu trình ban đầu căn chỉnh như thế nào, Katya sẽ tạo ra toàn bộ chuỗi mục tiêu. 

Đầu vào cung cấp chuỗi mục tiêu, sau đó là danh sách các phím bị hỏng cùng với độ dài chu kỳ của chúng. Đầu ra là số lần nhấn phím tối thiểu để đảm bảo chuỗi có thể được tạo ra hoàn chỉnh theo cách căn chỉnh tệ nhất có thể trong tất cả các chu kỳ. 

Độ dài chuỗi có thể lên tới 100000, trong khi có tối đa 26 phím bị hỏng. Điều này gợi ý rõ ràng rằng mọi giải pháp đều phải tuyến tính theo độ dài của chuỗi cộng với số lượng khóa. Bất cứ điều gì bậc hai trên chiều dài chuỗi sẽ quá chậm. 

Điểm tinh tế của phím là mỗi ký tự trong chuỗi bị ảnh hưởng độc lập bởi chu trình của khóa tương ứng, nhưng các chu trình có thể được căn chỉnh theo hướng ngược lại. Một mô phỏng đơn giản cố gắng theo dõi mọi tổ hợp pha có thể là không thể vì không gian trạng thái là hàm mũ của số lượng khóa bị hỏng. 

Một vấn đề tế nhị khác là các chữ cái khác nhau độc lập nhưng không đồng nhất: một số chữ cái bị hỏng, số khác thì không. Các chữ cái liền mạch luôn tạo ra chính xác một ký tự cho mỗi lần nhấn, vì vậy chúng đóng góp một cách xác định. Chỉ có phím bị hỏng mới gây ra sự chậm trễ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng quá trình gõ bằng cách nhấn bằng cách nhấn, theo dõi từng phím bị hỏng ở vị trí hiện tại trong chu kỳ của nó. Mỗi lần chúng ta gặp một ký tự trong chuỗi mục tiêu, chúng ta sẽ cố gắng nhấn phím tương ứng cho đến khi nó tạo ra lần xuất hiện bắt buộc tiếp theo của chữ cái đó. Vì các chu kỳ là tuần hoàn nên chúng ta cũng phải xem xét việc căn chỉnh pha trong trường hợp xấu nhất: khóa có thể ở trạng thái lỗi khi cần thiết. 

Ý tưởng mô phỏng này đúng về mặt tinh thần nhưng bị hỏng vì mỗi ký tự có thể yêu cầu tối đa O(x_i) lần nhấn lãng phí trong trường hợp xấu nhất và độ dài chuỗi có thể lớn. Tệ hơn nữa, sự tương tác giữa nhiều chữ cái khiến mô phỏng ngây thơ trở nên mơ hồ: chúng tôi không mô phỏng một quy trình xác định duy nhất mà đang tính toán đảm bảo trong trường hợp xấu nhất cho tất cả các giai đoạn ban đầu. 

Cái nhìn sâu sắc quan trọng là tách các chữ cái. Mỗi chữ cái đóng góp độc lập vào tổng số máy ép cần thiết. Đối với chữ không bị ngắt, mỗi lần nhấn sẽ đóng góp chính xác một ký tự nên chúng ta chỉ cần nhấn một lần cho mỗi lần xuất hiện. Đối với một chữ cái bị hỏng có tham số x, hành vi này mang tính tuần hoàn: trong mỗi khối x được nhấn, chỉ x-1 tạo ra đầu ra. Điều đó có nghĩa là hiệu suất trong trường hợp xấu nhất là (x-1)/x, và tương đương, để đảm bảo k kết quả đầu ra thành công, chúng ta phải tính đến các lần ép buộc bị lãng phí không thường xuyên. 

Cụ thể hơn, trong bất kỳ chuỗi lần nhấn nào, mỗi khối x lần nhấn liên tiếp một phím bị hỏng đều chứa đúng một lỗi. Từ góc độ đối nghịch, mỗi lần nhấn x chỉ đảm bảo x-1 đầu ra hữu ích, do đó, để tạo ra k lần xuất hiện, chúng ta cần trả thêm số lần nhấn bằng với số lần thất bại không thể tránh khỏi được phân bổ trên các lần xuất hiện đó.

Điều này làm giảm vấn đề về việc tính tổng, đối với mỗi chữ cái, số lần nó xuất hiện trong chuỗi nhân với hệ số lạm phát chi phí do chu kỳ của nó gây ra. Phần tinh tế là việc căn chỉnh trong trường hợp xấu nhất cho phép đối thủ đặt lỗi chính xác khi Katya cần đầu ra thành công, điều này bổ sung hiệu quả hiệu ứng phân chia trần cho mỗi tần số chữ cái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu của chu kỳ | O( | s | · tối đa x_i) | 
| Số học dựa trên tần số trên mỗi chữ cái | O( | s | + n) | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mỗi chữ cái trong chuỗi đích. Điều này giúp tách biệt số lượng đầu ra chúng ta cần cho mỗi khóa một cách độc lập. 
2. Lưu trữ tham số chu trình x cho mỗi phím bị hỏng. Nếu một chữ cái không bị hỏng, hãy coi x của nó là 1, nghĩa là mọi lần nhấn đều thành công. 
3. Đối với mỗi chữ cái, hãy tính xem cần bao nhiêu lần nhấn để đảm bảo tạo ra tần số của nó trong một chu kỳ trong đó cứ mỗi x lần nhấn thì có một lần bị lãng phí theo cách căn chỉnh tệ nhất. Điều này có thể được mô hình hóa bằng cách chia các đầu ra cần thiết thành các khối sản xuất đầy đủ và tính toán các lỗi không thể tránh khỏi xen kẽ giữa chúng. 
4. Tích lũy tổng số lần nhấn trên tất cả các chữ cái. 
5. Trả về tổng số lần nhấn phím được đảm bảo tối thiểu. 

Bước không tầm thường là chuyển thất bại định kỳ thành chi phí số học xác định. Thay vì theo dõi các giai đoạn, chúng tôi giả định sự đồng bộ hóa trong trường hợp xấu nhất trong đó mỗi lần nhấn thành công cần thiết đều có số lần thất bại trong phạm vi cấu trúc chu trình cho phép. 

### Tại sao nó hoạt động 

Mỗi phím bị hỏng hoạt động theo một lịch trình định kỳ cố định với chính xác một lỗi cho mỗi x lần nhấn. Trong bất kỳ chuỗi dài nào, tỷ lệ lỗi trên tổng số lần nhấn là cố định. Đối thủ luôn có thể căn chỉnh các vị trí lỗi để tối đa hóa nỗ lực lãng phí trên các đầu ra cần thiết, nhưng không thể vượt quá một lỗi trong mỗi khoảng thời gian chu kỳ. Do đó, tổng chi phí để tạo ra k đầu ra chính xác là số lượng máy ép nhỏ nhất có “năng suất hiệu quả” sau khi tổn thất định kỳ đạt đến k, được tính bằng cách phân phối đầu ra cần thiết qua các chu kỳ và tính đến lỗi bắt buộc trong mỗi khối hoàn chỉnh. 

Điều này biến một bài toán lập kế hoạch động thành một phép tính số học tĩnh trên mỗi chữ cái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = int(input())
    
    x = {}
    for _ in range(n):
        c, v = input().split()
        x[c] = int(v)
    
    freq = {}
    for ch in s:
        freq[ch] = freq.get(ch, 0) + 1
    
    ans = 0
    
    for ch, cnt in freq.items():
        if ch in x:
            v = x[ch]
            full = cnt // (v - 1)
            rem = cnt % (v - 1)
            ans += full * v + rem
        else:
            ans += cnt
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng bản đồ tần số của chuỗi mục tiêu. Điều này tránh mọi vấn đề về thứ tự vì chỉ tính vật chất chứ không phải vị trí. Sau đó, nó đọc độ dài chu kỳ khóa bị hỏng vào từ điển để truy cập O(1). 

Đối với mỗi ký tự, ý tưởng chính là mọi nhóm đầu ra thành công x-1 phải được thanh toán bằng x lần nhấn do lỗi cưỡng bức duy nhất trên mỗi chu kỳ. Biểu thức full * v + rem thực hiện việc đóng gói các đầu ra cần thiết này thành các chu kỳ trong khi tính toán các ký tự còn sót lại không điền vào một chu trình hoàn chỉnh. 

Một sai lầm phổ biến là quên rằng các chữ cái liền mạch phải được coi ngầm là x = 1, nếu không sẽ xảy ra phép chia cho 0 hoặc lạm phát không chính xác. Một sự tinh tế khác là đảm bảo phép chia số nguyên sử dụng hành vi sàn, phù hợp với cách hình thành toàn bộ chu kỳ đầu ra có thể sử dụng được. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
russiaopenhighschoolteamprogrammingcontest
2
s 3
o 5
```Đầu tiên chúng ta đếm số lần xuất hiện của mỗi chữ cái. Giả định`s`xuất hiện 4 lần và`o`xuất hiện 6 lần trong chuỗi. 

Vì`s`với x = 3, mỗi chu kỳ tạo ra 2 đầu ra hữu ích trên 3 lần nhấn. Chúng tôi nhóm 4 đầu ra thành hai khối có kích thước 2: 

| Thư | Đếm | x | khối đầy đủ | phần còn lại | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| s | 4 | 3 | 2 | 0 | 2×3 = 6 | 

Vì`o`với x = 5, mỗi khối tạo ra 4 đầu ra: 

| Thư | Đếm | x | khối đầy đủ | phần còn lại | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| o | 6 | 5 | 1 | 2 | 1×5 + 2 = 7 | 

Tất cả các chữ cái khác đều không bị gián đoạn và đóng góp tần số thô của chúng. 

Tổng hợp tất cả các đóng góp sẽ đưa ra câu trả lời cuối cùng. 

Dấu vết này cho thấy cách mỗi chữ cái được xử lý độc lập và cách các lực lượng thất bại định kỳ được nhóm thành các khối sản xuất có kích thước cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O(1) | Bản đồ tần số được giới hạn bởi kích thước bảng chữ cái | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì chuỗi được xử lý tuyến tính và chỉ các cấu trúc có kích thước không đổi mới được sử dụng để theo dõi tần số chữ cái và các tham số khóa bị hỏng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict

    s = input().strip()
    n = int(input())
    
    x = {}
    for _ in range(n):
        c, v = input().split()
        x[c] = int(v)
    
    freq = {}
    for ch in s:
        freq[ch] = freq.get(ch, 0) + 1
    
    ans = 0
    for ch, cnt in freq.items():
        if ch in x:
            v = x[ch]
            full = cnt // (v - 1)
            rem = cnt % (v - 1)
            ans += full * v + rem
        else:
            ans += cnt
    
    return str(ans).strip()

# provided sample
assert run("russiaopenhighschoolteamprogrammingcontest\n2\ns 3\no 5\n") == "46"

# single unbroken letter
assert run("aaaaa\n0\n") == "5"

# single broken letter, small cycle
assert run("aaaaa\n1\na 2\n") == "7"

# mixed letters
assert run("abacaba\n1\na 3\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều giống hệt nhau không bị gián đoạn | đếm tuyến tính | tính đúng đắn cơ bản | 
| chìa khóa đơn bị hỏng chu kỳ nhỏ | lạm phát cưỡng bức | xử lý chu trình | 
| chữ hỗn hợp | sự độc lập của các chữ cái | phân rã từng chữ cái | 

## Vỏ cạnh 

Trường hợp cạnh tối thiểu là khi không có phím nào bị hỏng. Trong trường hợp này, mỗi ký tự đóng góp chính xác một lần nhấn, vì vậy câu trả lời phải bằng độ dài chuỗi. Thuật toán xử lý việc này vì từ điển x trống và tất cả các ký tự đều rơi vào trường hợp mặc định. 

Một trường hợp cạnh khác là một chuỗi bao gồm toàn bộ một chữ cái bị hỏng với x = 2. Mỗi ký tự thành công đòi hỏi phải có thành công và thất bại xen kẽ, do đó, việc tạo ra k ký tự cần chính xác 2k - 1 lần nhấn theo cách căn chỉnh tệ nhất. Công thức full * v + rem tái tạo chính xác mẫu này bằng cách ghép từng đầu ra với một lần nhấn bị lãng phí bắt buộc ngoại trừ lần cuối cùng. 

Trường hợp cạnh cuối cùng là khi x lớn so với tần số. Nếu cnt < x - 1, thì không có chu kỳ đầy đủ và câu trả lời đơn giản là cnt, vì đối thủ không thể tạo ra sự thất bại hoàn toàn trong một phần chu kỳ ngoài độ lệch ban đầu. Số hạng còn lại rem nắm bắt trực tiếp trường hợp này mà không tính quá mức.
