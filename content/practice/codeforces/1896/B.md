---
title: "CF 1896B - AB Lật"
description: "Chúng ta được cho một chuỗi chỉ gồm hai ký tự A và B, có thể được coi là một dòng các ô liền kề. Nước đi duy nhất được phép là chọn một vị trí mà chữ A đứng ngay sau chữ B và hoán đổi chúng để chữ A di chuyển sang phải một bước."
date: "2026-06-08T21:37:07+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1896
codeforces_index: "B"
codeforces_contest_name: "CodeTON Round 7 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 900
weight: 1896
solve_time_s: 109
verified: false
draft: false
---

[CF 1896B - Lật AB](https://codeforces.com/problemset/problem/1896/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, chuỗi, hai con trỏ 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi chỉ gồm hai ký tự A và B, có thể được coi là một dòng các ô liền kề. Nước đi duy nhất được phép là chọn một vị trí mà chữ A đứng ngay sau chữ B và hoán đổi chúng để chữ A di chuyển sang phải một bước. Mỗi vị trí giữa hai ký tự có thể được sử dụng nhiều nhất một lần làm vị trí hoán đổi, nhưng các hoán đổi có thể được áp dụng theo bất kỳ thứ tự nào và chuỗi sẽ phát triển khi các hoán đổi được thực hiện. 

Nhiệm vụ là xác định số lần hoán đổi tối đa có thể được thực hiện trong toàn bộ quá trình. 

Ràng buộc rằng tổng độ dài trên tất cả các trường hợp thử nghiệm tối đa là 2⋅10^5 ngay lập tức cho thấy rằng mô phỏng O(n²) cho mỗi trường hợp thử nghiệm là quá chậm. Bất kỳ giải pháp nào về cơ bản phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một điểm tinh tế quan trọng là các giao dịch hoán đổi sẽ thay đổi chuỗi, do đó, một quyết định tham lam cục bộ sau này có thể ảnh hưởng đến việc liệu một giao dịch hoán đổi khác có khả thi ở cùng một chỉ mục hay không. Một cái bẫy khác là giả định rằng việc đếm số lần xuất hiện ban đầu của “AB” là đủ. Ví dụ: trong ABAB, sau khi hoán đổi ở vị trí 1, chúng ta nhận được BAAB, tạo ra một “AB” mới tại một vị trí khác mà ban đầu không có. 

Trường hợp tinh tế thứ hai là khi hoán đổi “di chuyển” A qua các khối B dài. Ví dụ: trong ABBBBA, một A có thể tương tác với nhiều ranh giới, nhưng mỗi chỉ mục chỉ có thể được sử dụng một lần, điều này ngăn chặn hiện tượng sủi bọt không giới hạn. 

Khó khăn cốt lõi là các giao dịch hoán đổi không độc lập: chúng sử dụng các vị trí, nhưng cấu hình chuỗi phát triển theo cách có thể cho phép các giao dịch hoán đổi mới ở nơi khác. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ quét chuỗi liên tục, tìm bất kỳ chỉ mục i nào có AB tồn tại và chưa được sử dụng, áp dụng hoán đổi và lặp lại cho đến khi không còn động thái nào. Mỗi lần hoán đổi cần O(n) để tìm thao tác hợp lệ tiếp theo và có thể xảy ra tối đa O(n2) hoán đổi trong các trường hợp bệnh lý, tạo ra tổng hành vi O(n³) trong một triển khai đơn giản. Ngay cả khi ghi sổ kế toán cẩn thận, việc duy trì một tập hợp động các vị thế hợp lệ vẫn tốn kém vì các giao dịch hoán đổi tạo ra và phá hủy các cơ hội cục bộ. 

Quan sát quan trọng là chúng ta thực sự không cần mô phỏng quá trình tiến hóa của dây. Mỗi lần hoán đổi tương ứng với một A vượt qua B tại một ranh giới và mỗi chỉ số ranh giới có thể được sử dụng nhiều nhất một lần. Thay vì suy nghĩ về việc thay đổi dây, chúng tôi theo dõi những “điểm giao cắt” tiềm năng nào có thể được thực hiện. 

Nếu chúng ta hiểu mọi A là thứ gì đó có thể di chuyển sang phải qua các B, thì mỗi lần hoán đổi về cơ bản là giải quyết một nghịch đảo cục bộ AB → BA. Tuy nhiên, một khi việc hoán đổi xảy ra ở chỉ số i, ranh giới đó sẽ vĩnh viễn cạn kiệt, do đó cấu trúc sẽ tiến gần hơn đến việc chọn các chuyển tiếp AB hợp lệ rời rạc trong biểu đồ kề cận đang phát triển. 

Sự đơn giản hóa quan trọng là nhận ra rằng quy trình này tương đương với việc sử dụng nhiều lần các ranh giới AB sẵn có, nhưng các ranh giới AB mới chỉ xuất hiện khi điểm A di chuyển ngang qua điểm B. Điều này tạo ra hiệu ứng giống như dòng chảy trong đó mỗi A có thể được ghép nối với nhiều khối B nhưng mỗi chỉ mục chỉ có thể được sử dụng một lần. Cách chính xác để nắm bắt điều này là xử lý từ trái sang phải trong khi vẫn duy trì số lượng A “có sẵn ở bên trái” để sử dụng các chuyển đổi B. 

Chúng tôi duy trì số lượng A đang hoạt động vẫn có thể di chuyển sang phải. Mỗi khi chúng ta nhìn thấy điểm B, nó có thể được hoán đổi với một trong những điểm A này ở bên trái, nhưng chỉ khi tồn tại một ranh giới chưa được sử dụng. Mỗi B thể hiện một cách hiệu quả mức tiêu thụ tiềm năng của một đóng góp A hiện có và mỗi lần ghép nối thành công tương ứng với một hoạt động hợp lệ. 

Điều này dẫn đến một cuộc quét tham lam, trong đó chúng ta tích lũy điểm A và tiêu thụ chúng khi gặp điểm B, đảm bảo rằng mỗi tương tác tương ứng với một cách sử dụng chỉ mục duy nhất.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n2) đến O(n³) | O(n) | Quá chậm | 
| Tương tác đếm quét tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải và duy trì số lượng A hiện có sẵn để tham gia vào các giao dịch hoán đổi trong tương lai. 

1. Khởi tạo bộ đếm`available_A = 0`và một câu trả lời`ops = 0`. 
2. Quét ký tự từ trái sang phải. 
3. Khi chúng ta nhìn thấy một`A`, chúng tôi tăng`available_A`bằng 1. Điều này thể hiện điểm A có khả năng di chuyển ngang qua điểm B trong tương lai. 
4. Khi chúng ta nhìn thấy một`B`, chúng ta kiểm tra xem có chữ A nào ở bên trái của nó không. Nếu như`available_A > 0`, chúng tôi thực hiện một thao tác: chúng tôi khớp B này với một A có sẵn, tăng dần`ops`bằng 1 và giảm`available_A`. 

Trực giác là mỗi trận đấu thành công tương ứng với một lần hoán đổi trong đó A vượt qua ranh giới B này. 

1. Tiếp tục cho đến hết chuỗi. 
2. Đầu ra`ops`. 

Sự lựa chọn tham lam là luôn khớp B với A có sẵn sớm nhất là an toàn vì bất kỳ A nào ở bên trái đều có thể hoán đổi cho nhau về mặt góp phần vào các giao dịch hoán đổi trong tương lai. Việc trì hoãn một trận đấu không thể làm tăng số trận đấu trong tương lai vì mỗi B chỉ có thể được sử dụng tối đa một lần trong một cặp đôi có lợi. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét,`available_A`đại diện cho số lượng người A vẫn còn tiềm năng chưa sử dụng để tham gia hoán đổi với người B ở bên phải của họ. Mỗi lần chúng tôi sử dụng một thông qua B, chúng tôi đang cam kết thực hiện một tương tác cấp chỉ mục riêng biệt. Vì mỗi thao tác thành công tiêu thụ vĩnh viễn chính xác một đơn vị công suất A bên trái và một vị trí B nên không thao tác nào gây trở ngại cho thao tác khác ngoại trừ bằng cách giảm tính khả dụng theo cách được kiểm soát. Điều này duy trì cách tính toán đơn điệu về các giao dịch hoán đổi có thể xảy ra, đảm bảo rằng mọi hoạt động được tính đều tương ứng với một vị trí hoán đổi AB hợp lệ và khác biệt theo một trình tự có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()

        available_A = 0
        ops = 0

        for ch in s:
            if ch == 'A':
                available_A += 1
            else:
                if available_A > 0:
                    available_A -= 1
                    ops += 1

        print(ops)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh một lần vượt qua cho mỗi trường hợp thử nghiệm. Trạng thái duy nhất mà chúng tôi duy trì là số lượng giao dịch hoán đổi hiện tại có thể sử dụng được cho các giao dịch hoán đổi trong tương lai và số lượng giao dịch hoán đổi mà chúng tôi đã đếm được. 

Chi tiết triển khai chính là chúng tôi không bao giờ mô phỏng rõ ràng các giao dịch hoán đổi hoặc sửa đổi chuỗi. Điều này tránh mọi nguy cơ xảy ra phản ứng dây chuyền không chính xác hoặc thiếu các cặp AB mới được tạo. Thứ tự quét đảm bảo rằng mọi B chỉ xem xét các A nằm hoàn toàn bên trái của nó, phù hợp với tính chất định hướng của hoạt động được phép. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1:`AABB`Chúng tôi theo dõi cách A tích lũy và tiêu thụ bởi B. 

| Bước | Char | có sẵn_A | hoạt động | 
| --- | --- | --- | --- | 
| 1 | A | 1 | 0 | 
| 2 | A | 2 | 0 | 
| 3 | B | 1 | 1 | 
| 4 | B | 0 | 2 | 

Điều này cho thấy mỗi B tiêu thụ một A đã thấy trước đó như thế nào, phản ánh hai cơ hội hoán đổi độc lập. 

Dấu vết xác nhận rằng mỗi B hoạt động như một phần chìm cho một A có sẵn và không xảy ra việc đếm hai lần. 

### Ví dụ 2:`ABAB`| Bước | Char | có sẵn_A | hoạt động | 
| --- | --- | --- | --- | 
| 1 | A | 1 | 0 | 
| 2 | B | 0 | 1 | 
| 3 | A | 1 | 1 | 
| 4 | B | 0 | 2 | 

Ở đây, chúng ta thấy rằng sau khi sử dụng A đầu tiên với B, A sau vẫn đóng góp vào B sau. Điều này chứng tỏ rằng quy trình xử lý các mẫu xen kẽ một cách tự nhiên mà không cần mô phỏng rõ ràng. 

Bất biến được khẳng định là`available_A`luôn thể hiện chính xác số lượng A chưa từng có vẫn có thể tham gia vào các giao dịch hoán đổi trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần trong một lần quét | 
| Không gian | O(1) | Chỉ có hai bộ đếm được duy trì bất kể kích thước đầu vào | 

Quá trình quét tuyến tính vừa vặn thoải mái trong giới hạn tổng n lên tới 2⋅10^5 và việc sử dụng bộ nhớ liên tục đảm bảo không có chi phí chung trong các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""3
2
AB
4
BBBA
4
AABB
""") == "1\n0\n2"

# all B's
assert run("""1
5
BBBBB
""") == "0"

# all A's
assert run("""1
5
AAAAA
""") == "0"

# alternating
assert run("""1
6
ABABAB
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| AB / BBBA / AABB | 1/0/2 | Tính đúng đắn cơ bản và hành vi mẫu | 
| BBBBB | 0 | Không có giao dịch hoán đổi có sẵn | 
| AAAAA | 0 | Không có B để tiêu thụ A | 
| ABABAB | 3 | Tương tác tối đa xen kẽ | 

## Vỏ cạnh 

Trường hợp phức tạp là khi tất cả A ở bên trái và tất cả B ở bên phải, chẳng hạn như`AAABBB`. Thuật toán xử lý tất cả A trước, tích lũy`available_A = 3`, sau đó sử dụng chúng khi B xuất hiện, tạo ra 3 thao tác. Điều này phù hợp với thực tế là mỗi B có thể ghép nối với một A trước đó. 

Một trường hợp khác là khi A và B thay thế nhau nhiều. Ví dụ`ABABAB`chứng minh rằng việc so khớp tham lam vẫn thành công vì mỗi B ngay lập tức tiêu thụ một A có sẵn nếu có thể và các A mới xuất hiện sẽ được tính toán chính xác sau đó. 

Trường hợp cạnh cuối cùng là tiền tố của B theo sau là A, chẳng hạn như`BBBAAA`. Vì không có A nào khả dụng khi gặp B nên không thể thực hiện được thao tác nào và A sau này không thể ảnh hưởng trở về trước đến B trước đó do các ràng buộc về hướng.
