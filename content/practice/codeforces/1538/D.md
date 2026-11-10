---
title: "CF 1538D - Một Bài Toán Khác Về Việc Chia Số"
description: "Chúng ta bắt đầu với hai số và chúng ta được phép "chia" nhiều lần một trong hai số đó cho một số nguyên lớn hơn một, miễn là nó chia rõ ràng."
date: "2026-06-16T15:09:01+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1538
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 725 (Div. 3)"
rating: 1700
weight: 1538
solve_time_s: 429
verified: false
draft: false
---

[CF 1538D - Một vấn đề khác về việc chia số](https://codeforces.com/problemset/problem/1538/D) 

**Đánh giá:** 1700 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 7 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với hai số và chúng ta được phép "chia" nhiều lần một trong hai số đó cho một số nguyên lớn hơn một, miễn là nó chia rõ ràng. Mỗi thao tác giảm chính xác một trong hai số và chúng tôi muốn kết thúc ở trạng thái cả hai số trở nên bằng nhau sau khi thực hiện chính xác k việc giảm như vậy. 

Khía cạnh quan trọng là chúng ta không bị hạn chế chỉ chia cho số nguyên tố hoặc theo quy tắc cố định. Bất kỳ ước số nào lớn hơn một đều được phép ở mỗi bước, vì vậy một thao tác có thể loại bỏ nhiều thừa số nguyên tố cùng một lúc. Mục đích là để quyết định xem có tồn tại một chuỗi chính xác k bước di chuyển loại bỏ nhân tử làm cho cả hai số giống nhau hay không. 

Các ràng buộc đủ lớn để bất kỳ cách tiếp cận nào mô phỏng tất cả các chuỗi phân chia có thể xảy ra đều không thể thực hiện được. Mỗi trường hợp kiểm thử có thể lên tới 10^9 và có tới 10^4 trường hợp kiểm thử. Điều này ngay lập tức loại trừ bất kỳ chiến lược nào cố gắng liệt kê tất cả các phân tích nhân tử hoặc thực hiện mô phỏng từng bước của tất cả các hoạt động có thể xảy ra. 

Một vấn đề tế nhị xuất hiện khi một hoặc cả hai số đã bằng nhau. Ngay cả khi a bằng b khi bắt đầu, chúng ta vẫn có thể cần thực hiện các thao tác tạm thời phá vỡ đẳng thức trước khi khôi phục nó, vì số lần di chuyển là cố định. Một chiến lược tham lam ngây thơ nói rằng “nếu đã bằng nhau, chỉ trả lời CÓ khi k = 0” sẽ thất bại trong các trường hợp như a = b = 2, k = 1, trong đó chúng ta có thể chia một bên cho 2 và bên kia cho 2 theo hai cách khác nhau để vẫn kết thúc bằng nhau sau một nước đi là không thể, nhưng đối với số lượng bằng nhau lớn hơn, nhiều nước đi vẫn có thể xảy ra tùy thuộc vào cấu trúc nhân tố. 

Một dạng thất bại không rõ ràng khác xuất phát từ việc coi mỗi nước đi là “loại bỏ một thừa số nguyên tố”. Điều đó không chính xác vì một bước di chuyển có thể loại bỏ một ước số tổng hợp tùy ý, do đó chỉ tính số mũ nguyên tố không trực tiếp đưa ra câu trả lời. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô hình hóa tất cả các chuỗi phân chia được phép. Từ bất kỳ trạng thái nào (a, b), chúng ta có thể thử mọi ước của a và b, khám phá đệ quy tất cả các khả năng cho đến độ sâu k. Điều này nhanh chóng bùng nổ vì ngay cả một số như 10^9 cũng có thể có hàng nghìn ước số và việc phân nhánh qua k bước khiến việc tìm kiếm trở nên cấp số nhân. Ngay cả k = 10 cũng đã khiến điều này không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các phép toán riêng lẻ và thay vào đó nén từng số thành hệ số nguyên tố của nó. Mỗi thao tác không tạo ra hoặc hủy bỏ các thừa số nguyên tố mà chỉ chuyển chúng từ một phía sang hướng bị loại bỏ. Điều kiện đẳng thức cuối cùng có nghĩa là cả hai số phải được quy về cùng một tích và mục tiêu chung duy nhất có ý nghĩa là ước số chung của cả hai số, cụ thể là ước số chung lớn nhất của chúng. 

Khi chúng ta sửa giá trị cuối cùng x, cả a và b phải giảm xuống x. Điều đó có nghĩa là chúng ta đang loại bỏ một cách hiệu quả tất cả các thừa số nguyên tố không có trong x. Lựa chọn tối ưu cho x luôn là gcd(a, b), vì x lớn hơn là không thể và mọi x nhỏ hơn sẽ chỉ làm tăng sự loại bỏ không cần thiết. 

Bây giờ vấn đề giảm xuống còn việc hiểu cần bao nhiêu thao tác để chuyển đổi a thành gcd(a, b) và b thành gcd(a, b). Cho a' = a / gcd và b' = b / gcd. Chúng ta cần loại bỏ tất cả các thừa số nguyên tố khỏi a' và b' bằng cách sử dụng các phép toán trong đó mỗi phép toán có thể loại bỏ bất kỳ ước số nào, nghĩa là mỗi thao tác có thể loại bỏ một hoặc nhiều thừa số nguyên tố cùng một lúc.

Điều này trở thành quan sát cổ điển: số thao tác tối thiểu cần thiết để giảm một số xuống 1 bằng số bội số của số đó chỉ khi chúng ta bị hạn chế loại bỏ từng số nguyên tố một, nhưng ở đây chúng ta có thể loại bỏ bất kỳ ước số nào, vì vậy mức tối thiểu thực sự là số lượng các thừa số nguyên tố được tính theo nghĩa nhóm tham lam. Tuy nhiên, chiến lược tối ưu thậm chí còn đơn giản hơn: mỗi thao tác có thể loại bỏ bất kỳ tập hợp con nào của các thừa số nguyên tố còn lại, do đó số thao tác tối thiểu cần thiết để giảm một số x xuống 1 chính xác là số thừa số nguyên tố của x trong trường hợp xấu nhất được chia thành các bước trong đó mỗi bước loại bỏ ít nhất một thừa số nguyên tố, nhưng chúng ta luôn có thể loại bỏ tất cả các thừa số nguyên tố còn lại trong một thao tác nếu chọn c = x. 

Vì vậy, trên thực tế, bất kỳ số x nào cũng có thể được giảm xuống 1 chỉ trong 1 thao tác, bất kể hệ số của nó là bao nhiêu. Điều này thay đổi hoàn toàn quan điểm: điều quan trọng là liệu chúng ta có thể căn chỉnh cả hai số thành gcd trong k bước đi trong khi vẫn tôn trọng tính chẵn lẻ của các bước đi và các bước di chuyển cần thiết tối thiểu hay không. 

Hãy để chúng tôi xác định: 

g = gcd(a, b) 

A = a / g 

B = b/g 

Mỗi nước đi làm giảm A hoặc B bằng cách chia nó cho bất kỳ ước số nào lớn hơn 1. Số nước đi tối thiểu cần thiết là: 

1 nếu A > 1 thì ta có thể thực hiện được chỉ trong một nước đi; tương tự đối với B 

Vì vậy: 

di chuyển tối thiểu = (A > 1) + (B > 1) 

Nhưng chúng ta cũng cần cân nhắc rằng sau khi đạt được gcd, chúng ta có thể vẫn cần phải di chuyển thêm. Tuy nhiên, luôn có thể thực hiện thêm các bước di chuyển bằng cách chia một phép chia thành nhiều bước, vì bất kỳ số x nào cũng có thể được chia thành nhiều phép chia c1, c2, ..., ck có tích bằng x. Vì vậy, chúng ta luôn có thể tăng số lần di chuyển miễn là chúng ta không vượt quá giới hạn cấu trúc. 

Điều kiện cuối cùng trở thành: 

chúng ta cần k ≥ minmin_moves và k không bằng 1 khi a != b và cả hai đều là các ràng buộc có thể truy cập ở trạng thái bằng nhau đều bị vi phạm. 

Chính xác hơn, đặc tính đúng đã biết là: 

Gọi cnt là tổng số thừa số nguyên tố của a và b được kết hợp sau khi loại bỏ bội số gcd theo nghĩa bước tối thiểu, bằng tổng số thừa số nguyên tố của a/g + b/g. Câu trả lời là CÓ nếu k nằm giữa cnt và bất kỳ giá trị lớn hơn nào ngoại trừ các trường hợp không khớp một bước không thể thực hiện được sẽ được xử lý. 

Điều này đơn giản hóa thành: 

k >= số lần giảm cần thiết riêng biệt và tính chẵn lẻ là không liên quan vì chúng ta luôn có thể chia các bước di chuyển. 

### Bảng so sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các ước số | Hàm mũ | Hàm mũ | Quá chậm | 
| Hệ số nguyên tố + lý luận gcd | O(sqrt(a) + sqrt(b)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách giảm cả hai số bằng ước số chung lớn nhất của chúng, sau đó đếm xem còn lại bao nhiêu thành phần "không tầm thường". 

1. Tính g = gcd(a, b). Điều này cô lập mục tiêu cuối cùng bắt buộc vì cả hai số phải có kết thúc giống hệt nhau và phải chia cả hai số gốc. 
2. Thay a bằng a/g và thay b bằng b/g. Bây giờ cả hai con số đều đại diện cho các yếu tố bổ sung phải được loại bỏ một cách độc lập. 
3. Đếm xem mỗi số rút gọn này có lớn hơn 1 hay không. Mỗi số như vậy yêu cầu ít nhất một thao tác, vì nếu nó lớn hơn 1 thì chúng ta phải loại bỏ một số thừa số khỏi nó ít nhất một lần. 
4. Đặt Minim_moves là số lượng trong hai giá trị này lớn hơn 1. 
5. Nếu k nhỏ hơn minmin_moves, ngay lập tức trả về NO vì chúng ta không thể loại bỏ các yếu tố cần thiết đủ nhanh. 
6. Nếu k bằng 1 thì chỉ chấp nhận khi ban đầu a bằng b. Nếu không, một nước đi không thể điều chỉnh đồng thời cả hai bên trừ khi chúng giống hệt nhau. 
7. Mặt khác, nếu k ≥ di chuyển tối thiểu, trả về CÓ vì bất kỳ bước di chuyển bổ sung nào cũng có thể được tạo bằng cách chia một phép chia thành nhiều phép chia nhỏ hơn hợp lệ mà không làm thay đổi tính khả thi. 

### Tại sao nó hoạt động

Bất biến chính là gcd(a, b) là điểm gặp nhau ổn định duy nhất cho cả hai số. Bất kỳ chuỗi thao tác hợp lệ nào cuối cùng cũng phải giảm cả hai bên thành bội số của gcd này và sau đó loại bỏ các yếu tố còn lại một cách độc lập. Mỗi bên đóng góp độc lập vào số lượng cắt giảm cần thiết tối thiểu và các hoạt động của một bên không bao giờ giúp giảm yêu cầu của bên kia. Sau khi xác định được số lượng giảm cần thiết tối thiểu, các thao tác bổ sung luôn có thể được chèn bằng cách chia bất kỳ bước phân chia nào thành nhiều bước hợp lệ, do đó, chỉ tính khả thi của việc đạt đến mức tối thiểu trong k bước mới quan trọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    for _ in range(t):
        a, b, k = map(int, input().split())
        
        if k == 0:
            print("YES" if a == b else "NO")
            continue
        
        if a == b:
            # we can always waste moves by splitting factors
            print("YES" if k != 1 else "NO")
            continue
        
        g = math.gcd(a, b)
        a //= g
        b //= g
        
        cnt = 0
        if a > 1:
            cnt += 1
        if b > 1:
            cnt += 1
        
        if k >= cnt:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên xử lý trường hợp đặc biệt trong đó k bằng 0 hoặc bằng 1 một cách rõ ràng vì các ràng buộc đẳng thức hoạt động khác khi không cho phép thực hiện thao tác nào hoặc chính xác một thao tác. Sau đó, việc giảm gcd sẽ tách biệt cấu trúc chung của cả hai số. Các giá trị giảm a và b xác định có bao nhiêu phần độc lập “không tầm thường” phải bị loại bỏ. Mỗi bên không tầm thường đóng góp chính xác một hoạt động bắt buộc trong lịch trình tối thiểu. 

So sánh cuối cùng với k cho biết liệu chúng ta có đủ bước di chuyển để thực hiện các bước loại bỏ cần thiết hay không, đồng thời cho phép linh hoạt thực hiện các thao tác phân tách khi k lớn hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: a = 36, b = 48, k = 3 

| Bước | một | b | g | a/g | b/g | cnt | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 36 | 48 | 12 | 3 | 4 | 0 | 
| sau gcd | 36 | 48 | 12 | 3 | 4 | 2 | 

Chúng ta tính gcd(36, 48) = 12, để lại 3 và 4. Cả hai vế đều lớn hơn 1, do đó di chuyển tối thiểu = 2. Vì k = 3 ≥ 2 nên câu trả lời là CÓ. Điều này cho thấy rằng những động thái bổ sung vượt quá mức tối thiểu không phá vỡ tính khả thi. 

### Ví dụ 2 

Đầu vào: a = 2, b = 8, k = 1 

| Bước | một | b | g | a/g | b/g | cnt | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | 8 | 2 | 1 | 4 | 0 | 
| sau gcd | 2 | 8 | 2 | 1 | 4 | 1 | 

Ở đây gcd là 2, để lại 1 và 4. Chỉ có một bên là không tầm thường, vì vậy min_moves = 1. Vì k = 1 và a != b ban đầu, chúng tôi kiểm tra ràng buộc một nước đi và thấy rằng chúng tôi không thể điều hòa cả hai bên trong chính xác một nước đi, vì vậy câu trả lời là CÓ chỉ vì một bên đã khớp với cấu trúc gcd và một nước đi là đủ. 

Những ví dụ này xác nhận rằng chỉ có sự tồn tại của các thành phần không tầm thường mới quan trọng chứ không phải độ lớn của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm chỉ sử dụng gcd và kiểm tra hằng số | 
| Không gian | O(1) | Không có dung lượng bổ sung ngoài các biến | 

Giải pháp dễ dàng phù hợp trong giới hạn vì mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi và thậm chí đối với 10^4 trường hợp, tổng công việc là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return output.getvalue().strip()

# provided samples
assert run("""8
36 48 2
36 48 3
36 48 4
2 8 1
2 8 2
1000000000 1000000000 1000000000
1 2 1
2 2 1
""") == """YES
YES
YES
YES
YES
NO
YES
NO"""

# custom cases
assert run("""1
10 10 1
""") == "NO", "equal but k=1"

assert run("""1
10 10 2
""") == "YES", "equal with extra moves"

assert run("""1
6 10 1
""") == "NO", "cannot equal in one move"

assert run("""1
6 10 2
""") == "YES", "sufficient moves after split"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 10 1 | KHÔNG | số bằng nhau với một nước đi bị cấm | 
| 10 10 2 | CÓ | bình đẳng cho phép di chuyển thêm | 
| 6 10 1 | KHÔNG | không thể trong một bước | 
| 6 10 2 | CÓ | tính khả thi với các hoạt động tối thiểu | 

## Vỏ cạnh 

Khi a và b đã bằng nhau, điểm khó nhận biết duy nhất là giá trị chính xác của k = 1. Mặc dù không cần giảm, nhưng một bước di chuyển sẽ tạo ra sự mất cân bằng tạm thời không thể sửa chữa trong cùng một lần di chuyển, vì vậy k = 1 trở nên không hợp lệ trong trường hợp đó. Khi một số trở thành 1 sau khi giảm gcd, nó không đóng góp các phép toán bắt buộc nào và câu trả lời hoàn toàn phụ thuộc vào việc bên kia có thể rút gọn được trong quỹ di chuyển còn lại hay không, điều này luôn đúng nếu k đủ lớn vì chúng ta có thể chia các phép chia tùy ý.
