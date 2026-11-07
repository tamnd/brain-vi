---
title: "CF 1341A - Nastya và gạo"
description: "Chúng ta được đưa ra một số tình huống độc lập về một bao gạo. Trong mỗi kịch bản, có hai lớp không chắc chắn: trọng lượng của từng hạt riêng lẻ và tổng trọng lượng của tất cả các hạt cộng lại."
date: "2026-06-15T04:39:36+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1341
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 637 (Div. 2) - Thanks, Ivan Belonogov!"
rating: 900
weight: 1341
solve_time_s: 77
verified: true
draft: false
---

[CF 1341A - Nastya và Rice](https://codeforces.com/problemset/problem/1341/A) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một số tình huống độc lập về một bao gạo. Trong mỗi kịch bản, có hai lớp không chắc chắn: trọng lượng của từng hạt riêng lẻ và tổng trọng lượng của tất cả các hạt cộng lại. 

Mỗi hạt được biết là có trọng lượng nguyên nằm trong một khoảng đối xứng xung quanh một giá trị trung bình nào đó. Riêng biệt, tổng trọng lượng của tất cả các hạt cũng bị hạn chế nằm trong một khoảng đối xứng khác. Câu hỏi đặt ra là liệu có tồn tại bất kỳ sự gán trọng số nguyên nào cho tất cả các hạt sao cho mỗi hạt tôn trọng phạm vi cho phép của chính nó và tổng của tất cả các hạt nằm trong tổng phạm vi cho phép hay không. 

Chúng ta không được yêu cầu xây dựng các trọng số mà chỉ quyết định xem cấu hình như vậy có khả thi về mặt logic hay không. 

Các ràng buộc là nhỏ đối với mỗi trường hợp thử nghiệm, lên tới 1000 hạt và tối đa 1000 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng phải có thời gian không đổi cho mỗi trường hợp thử nghiệm, vì ngay cả việc quét tuyến tính cho mỗi trường hợp cũng đã đạt được khoảng 10^6 thao tác, điều này vẫn ổn, nhưng bất kỳ điều gì liên quan đến lý luận lồng nhau trong phân phối sẽ là không cần thiết và quá mức cần thiết. 

Một cạm bẫy tinh vi đến từ việc chỉ nghĩ về mức trung bình. Một ý tưởng ngây thơ là so sánh phạm vi trọng lượng hạt trung bình với mức trung bình được ngụ ý trong tổng khoảng thời gian. Điều này không chính xác vì tổng ràng buộc là ở tổng chứ không phải ở mức trung bình trên mỗi hạt và các cấu hình cực trị rất quan trọng. 

Một sai lầm tiềm ẩn khác là xử lý các giới hạn một cách độc lập mà không chia tỷ lệ. Tổng phạm vi có tỷ lệ với n, nhưng việc triển khai bất cẩn có thể quên nhân các giới hạn trên mỗi hạt, dẫn đến việc kiểm tra tính khả thi không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ là thử tất cả các phép gán trọng lượng hạt có thể có trong phạm vi cho phép của chúng và kiểm tra xem liệu phép gán nào có tạo ra tổng trong khoảng yêu cầu hay không. Ngay cả đối với một trường hợp thử nghiệm đơn lẻ, mỗi hạt có khoảng 2001 giá trị có thể, do đó số lượng kết hợp theo thứ tự$(2001)^n$, điều này hoàn toàn không thể thực hiện được ngay cả với n rất nhỏ. Cấu trúc của bài toán cho thấy chúng ta không cần phải kiểm tra từng bài tập riêng lẻ. 

Quan sát quan trọng là chúng ta không quan tâm đến việc phân bổ chính xác các trọng số mà chỉ quan tâm đến phạm vi có thể có của tổng. Vì mỗi hạt là độc lập và đóng góp bổ sung, tổng số tối thiểu có thể đạt được khi mỗi hạt có giá trị tối thiểu và tổng số tối đa có thể đạt được khi mỗi hạt có giá trị tối đa. Điều này biến toàn bộ vấn đề thành việc kiểm tra xem hai khoảng có trùng nhau hay không: khoảng tổng có thể đạt được và tổng khoảng cần thiết. 

Khi chúng tôi tính toán phạm vi tổng có thể đạt được, tính khả thi sẽ giảm xuống việc kiểm tra xem hai khoảng này có giao nhau hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Lý luận khoảng thời gian | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi trường hợp thử nghiệm, chúng tôi tiến hành một số bước xác định nhỏ. 

1. Tính trọng lượng tối thiểu có thể có của một hạt như sau:$a - b$. Đây là giá trị được phép nhỏ nhất trên mỗi hạt. 
2. Tính trọng lượng tối đa có thể có của một hạt như sau:$a + b$. Đây là giá trị được phép lớn nhất trên mỗi hạt. 
3. Nhân cả hai với$n$để có được tổng trọng lượng tối thiểu và tối đa có thể:$S_{\min} = n(a - b)$Và$S_{\max} = n(a + b)$. Bước này hợp lý vì mỗi hạt đóng góp độc lập và chúng ta có thể chọn từng hạt ở cực trị của nó để đẩy tổng. 
4. Tính tổng khoảng trọng số cho phép từ đầu vào như sau:$[c - d, c + d]$. 
5. Kiểm tra xem hai khoảng này có giao nhau không. Cụ thể, chúng tôi xác minh rằng số tiền tối đa có thể đạt được ít nhất là tổng số tối thiểu được phép và số tiền tối thiểu có thể đạt được nhiều nhất là tổng số tối đa được phép. 
6. Nếu cả hai điều kiện đều giữ nguyên, xuất "Có"; nếu không thì xuất ra "Không". 

### Tại sao nó hoạt động 

Tổng các biến giới hạn độc lập luôn đạt cực trị bằng cách đẩy mọi biến tới cực trị của chính nó. Bất kỳ sai lệch nào so với việc chọn tất cả các mức tối thiểu đều làm tăng tổng và bất kỳ sai lệch nào so với việc chọn tất cả các mức tối đa đều làm giảm tổng, do đó không có cấu hình trung gian nào có thể mở rộng khoảng có thể tiếp cận vượt quá$[n(a-b), n(a+b)]$. Do đó, vấn đề quy về việc kiểm tra xem khoảng thời gian có thể đạt được này có trùng với khoảng thời gian cần thiết hay không, điều này vừa cần thiết vừa đủ cho tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, a, b, c, d = map(int, input().split())
    
    min_sum = n * (a - b)
    max_sum = n * (a + b)
    
    low = c - d
    high = c + d
    
    if max_sum >= low and min_sum <= high:
        print("Yes")
    else:
        print("No")
```Việc thực hiện trực tiếp tuân theo lý luận khoảng thời gian xuất phát. Chi tiết chính là chia tỷ lệ chính xác cả hai điểm cuối của khoảng cách trên mỗi hạt theo n. Một sai lầm thường gặp là so sánh$a-b$Và$a+b$trực tiếp với tổng giới hạn mà không nhân lên, bỏ qua việc tổng hợp. 

Điều kiện chồng chéo là đối xứng: một trong hai điểm cuối có thể xác thực tính khả thi, nhưng cả hai phải được kiểm tra để đảm bảo các khoảng giao nhau thay vì một khoảng hoàn toàn chứa điểm kia. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp mẫu để xem cách so sánh khoảng thời gian hoạt động như thế nào. 

### Ví dụ 1 

đầu vào:`7 20 3 101 18`| Bước | hạt tối thiểu | hạt tối đa | tổng tối thiểu | tổng tối đa | tổng khoảng thời gian | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tính toán | 17 | 23 | 119 | 161 | [83, 119] | Có | 

Phạm vi tổng có thể đạt được là [119, 161], trong khi phạm vi yêu cầu là [83, 119]. Các khoảng chạm tới 119, do đó tính khả thi giữ chính xác ở trường hợp biên trong đó tất cả các hạt đều có giá trị tối thiểu. 

### Ví dụ 2 

đầu vào:`11 11 10 234 2`| Bước | hạt tối thiểu | hạt tối đa | tổng tối thiểu | tổng tối đa | tổng khoảng thời gian | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tính toán | 1 | 21 | 11 | 231 | [232, 236] | Không | 

Ở đây tổng tối đa có thể là 231, nhưng khoảng thời gian bắt buộc bắt đầu từ 232. Ngay cả cấu hình thuận lợi nhất cũng không thể đạt được tổng yêu cầu. 

Những ví dụ này cho thấy rằng việc chạm vào ranh giới là hợp lệ và việc phân tách nghiêm ngặt sẽ gây ra lỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm sử dụng các phép toán số học không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ tỷ lệ thuận với kích thước đầu vào | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc vì ngay cả 1000 trường hợp thử nghiệm cũng chỉ yêu cầu một vài phép tính số học cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n, a, b, c, d = map(int, input().split())
        min_sum = n * (a - b)
        max_sum = n * (a + b)
        low = c - d
        high = c + d
        out.append("Yes" if (max_sum >= low and min_sum <= high) else "No")
    return "\n".join(out)

# provided samples
assert run("""5
7 20 3 101 18
11 11 10 234 2
8 9 7 250 122
19 41 21 321 10
3 10 8 6 1
""") == """Yes
No
Yes
No
Yes"""

# minimum case
assert run("1\n1 2 1 1 0\n") == "Yes"

# impossible small case
assert run("1\n1 10 1 5 0\n") == "No"

# tight boundary overlap
assert run("1\n2 5 1 9 1\n") == "Yes"

# large n edge
assert run("1\n1000 1000 0 1000000 0\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chồng chéo hạt đơn | Có | độ đúng ranh giới tối thiểu | 
| hạt đơn không thể | Không | phát hiện không chồng chéo | 
| chồng chéo chặt chẽ | Có | xử lý ngã tư ranh giới | 
| lớn | Có | độ chính xác của tỷ lệ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng xảy ra khi các khoảng chạm chính xác vào một điểm. Ví dụ: nếu số tiền tối đa có thể đạt được bằng số tiền tối thiểu cần thiết thì câu trả lời đúng vẫn là "Có". Thuật toán xử lý việc này vì điều kiện sử dụng các bất đẳng thức không nghiêm ngặt, đảm bảo tính khả thi về biên được chấp nhận. 

Một trường hợp khác là khi n bằng 1. Vấn đề giảm xuống còn việc kiểm tra xem hai khoảng có trùng nhau trực tiếp hay không và bước chia tỷ lệ vẫn hoạt động chính xác vì nhân với 1 sẽ bảo toàn khoảng. 

Trường hợp thứ ba là khi b bằng 0 hoặc d bằng 0. Điều này thu gọn một hoặc cả hai khoảng thành các điểm đơn. Thuật toán vẫn hoạt động vì việc kiểm tra giao lộ xử lý các khoảng suy biến một cách tự nhiên mà không cần đặt vỏ đặc biệt.
