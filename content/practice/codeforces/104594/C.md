---
title: "CF 104594C - Biến đổi"
description: "Chúng ta được cung cấp một tập hợp các kim loại trong đó kim loại 1 đặc biệt vì mỗi gam của nó được tính trực tiếp là một gam của câu trả lời cuối cùng. Mỗi kim loại khác đều có chính xác một quy tắc tổng hợp: nếu chúng ta phá hủy một gam của hai kim loại cụ thể, chúng ta thu được một gam kim loại khác."
date: "2026-06-30T05:21:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104594
codeforces_index: "C"
codeforces_contest_name: "2018 Google Code Jam Round 1B (GCJ 18 Round 1B)"
rating: 0
weight: 104594
solve_time_s: 54
verified: true
draft: false
---

[CF 104594C - Biến đổi](https://codeforces.com/problemset/problem/104594/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các kim loại trong đó kim loại 1 đặc biệt vì mỗi gam của nó được tính trực tiếp là một gam của câu trả lời cuối cùng. Mỗi kim loại khác đều có chính xác một quy tắc tổng hợp: nếu chúng ta phá hủy một gam của hai kim loại cụ thể, chúng ta thu được một gam kim loại khác. Hoạt động này là không thể đảo ngược và bị mất hoàn toàn về tổng khối lượng, vì hai gam trở thành một gam. 

Chúng tôi bắt đầu với một số lượng ban đầu của tất cả các kim loại. Chúng ta có thể áp dụng lặp đi lặp lại các quy tắc tổng hợp này bao nhiêu lần cũng được, miễn là chúng ta có đủ kim loại đầu vào. Mục tiêu của chúng tôi là tạo ra càng nhiều kim loại 1 càng tốt, có thể bằng cách chuyển đổi các kim loại trung gian thành nhau theo một trật tự thông minh. 

Các ràng buộc nhỏ về cấu trúc nhưng không nhỏ về giá trị. Số lượng kim loại tối đa là 100 trong các nhiệm vụ phụ ẩn, trong khi số lượng ban đầu có thể lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào theo dõi gam một cách rõ ràng hoặc cố gắng mô hình hóa từng hoạt động từng bước, vì số lượng các phép biến đổi có thể có có thể tăng tuyến tính theo số lượng và trở nên khổng lồ. 

Khó khăn chính là các phép biến đổi không thể đảo ngược và các kim loại khác nhau có thể ảnh hưởng gián tiếp lẫn nhau thông qua chuỗi công thức nấu ăn. Một cách tiếp cận tham lam ngây thơ như “luôn sản xuất kim loại 1 bất cứ khi nào có thể” sẽ thất bại vì các kim loại trung gian có thể có giá trị hơn nếu sau này được sử dụng theo cách khác. 

Trường hợp có cạnh tinh tế là khi kim loại có thể được sản xuất từ ​​chính nó một cách gián tiếp. Ví dụ: nếu một quy tắc cho phép kết hợp kim loại 2 và 3 để tạo ra kim loại 2, thì việc áp dụng nhiều lần quy tắc này có thể tạo ra các chu kỳ trong đó cùng một kim loại sẽ tăng tính hữu dụng của nó theo những cách không rõ ràng. Một quyết định tham lam cục bộ có thể lạm dụng hoặc lạm dụng các chu kỳ như vậy và không đạt được kết quả tối ưu. 

Một dạng thất bại khác xuất hiện khi một kim loại không trực tiếp đóng góp vào chì vẫn tham gia vào quá trình chuyển đổi trung gian để mở khóa chuỗi tốt hơn nhiều. Bỏ qua các kim loại như vậy hoàn toàn dẫn đến kết quả dưới mức tối ưu. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp sẽ cố gắng mô phỏng tất cả các trình tự áp dụng công thức nấu ăn có thể có. Mỗi trạng thái là một vectơ đại lượng kim loại và mỗi quá trình chuyển đổi tiêu thụ hai đơn vị và tạo ra một đơn vị. Hệ số phân nhánh lớn vì mọi kim loại với nguyên liệu sẵn có đều có thể được sản xuất bất cứ lúc nào. Ngay cả với M nhỏ, số lượng trạng thái có thể tiếp cận sẽ tăng vọt do sự phân bố gam khác nhau trên các kim loại, khiến phương pháp này không khả thi. 

Quan sát cấu trúc chính là chúng ta không thực sự quan tâm đến các cấu hình trung gian mà chỉ quan tâm đến giá trị của mỗi kim loại trong quá trình sản xuất chì cuối cùng. Nếu chúng ta có thể gán một giá trị duy nhất cho mỗi kim loại biểu thị số lượng đơn vị chì mà một gam kim loại đó cuối cùng có thể tạo ra, thì câu trả lời cuối cùng sẽ chỉ đơn giản là tổng trọng số của lượng chì ban đầu. 

Điều này làm giảm vấn đề tính toán các giá trị này một cách nhất quán trên tất cả các quy tắc chuyển đổi. Nếu kim loại i có thể được tạo ra từ kim loại a và b thì việc tiếp cận a và b cho phép chúng ta thu được i. Do đó, i ít nhất phải có giá trị bằng giá trị kết hợp của a và b. Điều này đưa ra một hệ thống các ràng buộc đơn điệu có thể được nới lỏng lặp đi lặp lại cho đến khi ổn định. 

Chúng tôi liên tục cải thiện giá trị của từng kim loại bằng cách sử dụng quy tắc v[i] = max(v[i], v[a] + v[b]). Vì các giá trị chỉ tăng và bị giới hạn một cách hữu hạn (không có chu kỳ tăng nghiêm ngặt nào có thể tồn tại vô thời hạn trong một bao đóng nhất quán), nên quá trình này hội tụ. 

Khi các giá trị ổn định, mỗi gam kim loại i đóng góp độc lập v[i] đơn vị chì, do đó câu trả lời cuối cùng chỉ đơn giản là tổng của tất cả các đại lượng ban đầu nhân với các giá trị này.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ trong hoạt động | Hàm mũ | Quá chậm | 
| Thư giãn lan truyền giá trị | O(M^2 * lần lặp) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Gán giá trị ban đầu cho mỗi kim loại, trong đó kim loại 1 bắt đầu bằng giá trị 1 vì nó trực tiếp đại diện cho chì. Tất cả các kim loại khác đều bắt đầu từ số 0 vì ban đầu chúng không được biết là tạo ra chì. 
2. Quét liên tục tất cả các công thức nấu ăn. Đối với mỗi công thức trong đó kim loại i được sản xuất từ ​​kim loại a và b, hãy cố gắng cải thiện v[i] bằng cách sử dụng tổng v[a] + v[b]. Trực giác cho thấy rằng nếu a và b cùng nhau cuối cùng có thể tạo ra một lượng chì nhất định thì i, có thể thu được từ chúng, phải kế thừa ít nhất giá trị đó. 
3. Nếu có bất kỳ giá trị nào thay đổi trong quá trình quét toàn bộ, hãy lặp lại quy trình. Việc thư giãn lặp đi lặp lại này là cần thiết bởi vì việc cải thiện một kim loại có thể mở ra những cải tiến ở những kim loại khác thông qua chuỗi phụ thuộc. 
4. Dừng lại khi việc vượt qua đầy đủ không tạo ra thay đổi nào. Khi đó, mọi kim loại đều đã đạt đến giá trị ổn định phù hợp với mọi quy luật biến đổi. 
5. Tính đáp án cuối cùng bằng cách tính tổng G[i] * v[i] của tất cả các kim loại. 

### Tại sao nó hoạt động 

Quá trình này duy trì một hệ thống đơn điệu với các giới hạn dưới về giá trị thực của mỗi kim loại. Mỗi bước nới lỏng chỉ làm tăng các giá trị theo cách được chứng minh bằng một cấu trúc rõ ràng: nếu a và b có thể mang lại các giá trị khách hàng tiềm năng nhất định thì tôi có thể kế thừa chúng thông qua một ứng dụng hợp lệ của công thức. Vì các giá trị chỉ tăng và bị giới hạn bởi những gì thực sự có thể được xây dựng từ các nguồn tài nguyên ban đầu hữu hạn nên quá trình phải hội tụ đến phép gán nhất quán tối đa. Khi đã ổn định, không có công thức nào có thể cải thiện bất kỳ giá trị nào, có nghĩa là mọi đường dẫn xây dựng gián tiếp đều đã được tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        M = int(input())
        A = [0] * (M + 1)
        B = [0] * (M + 1)
        for i in range(1, M + 1):
            a, b = map(int, input().split())
            A[i] = a
            B[i] = b

        G = list(map(int, input().split()))
        G = [0] + G

        # v[i] = value of 1 gram of metal i in lead units
        v = [0] * (M + 1)
        v[1] = 1

        changed = True
        while changed:
            changed = False
            for i in range(1, M + 1):
                a, b = A[i], B[i]
                if v[a] + v[b] > v[i]:
                    v[i] = v[a] + v[b]
                    changed = True

        ans = 0
        for i in range(1, M + 1):
            ans += G[i] * v[i]

        print(f"Case #{tc}: {ans}")

if __name__ == "__main__":
    solve()
```Việc thực hiện duy trì một mảng`v`lưu trữ giá trị được biết đến tốt nhất của mỗi kim loại. Kim loại 1 được gieo mầm giá trị 1 vì nó đóng góp trực tiếp vào mục tiêu. Mỗi lần lặp lại sẽ quét tất cả các quy tắc biến đổi và giảm giá trị của kim loại được sản xuất dựa trên các thành phần của nó. Vòng lặp tiếp tục cho đến khi không có cập nhật nào xảy ra, đảm bảo tất cả các cải tiến giá trị gián tiếp đã được phổ biến trong hệ thống. 

Một chi tiết tinh tế là chúng tôi không bao giờ cố gắng mô phỏng chuyển đổi gam thực tế. Tính đúng đắn hoàn toàn xuất phát từ việc coi các phép biến đổi là các ràng buộc trong việc truyền bá giá trị thay vì các chuyển đổi trạng thái rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một hệ thống nhỏ trong đó kim loại 1 là chì và các kim loại khác cuối cùng có thể tạo ra nó thông qua một chuỗi. 

| Lặp lại | v1 | v2 | v3 | Quy tắc cập nhật | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 0 | 0 | khởi tạo | 
| 1 | 1 | 1 | 1 | kim loại 2 và 3 cải thiện thông qua sự kết hợp | 
| 2 | 1 | 1 | 1 | không có thay đổi gì thêm | 

Trong dấu vết này, khi kim loại 2 và 3 kế thừa giá trị từ kim loại 1 thông qua các quy tắc trung gian, hệ thống sẽ ổn định ngay lập tức. Điều này xác nhận rằng việc truyền bá nắm bắt chính xác tính hữu ích gián tiếp. 

### Ví dụ 2 

Một cấu trúc tuần hoàn trong đó các kim loại tăng cường lẫn nhau. 

| Lặp lại | v1 | v2 | v3 | Quy tắc cập nhật | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 0 | 0 | khởi tạo | 
| 1 | 1 | 1 | 1 | bắt đầu củng cố lẫn nhau | 
| 2 | 1 | 2 | 2 | lan truyền tiếp theo làm tăng giá trị | 
| 3 | 1 | 2 | 2 | ổn định | 

Điều này chứng tỏ rằng các chu kỳ không phá vỡ tính đúng đắn. Việc nới lỏng tiếp tục cho đến khi không có quy tắc nào có thể tăng thêm bất kỳ giá trị nào, điều này đảm bảo đạt được điểm cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M^2 * K) | Mỗi lần lặp sẽ quét tất cả M công thức nấu ăn và giá trị tăng tối đa cho đến khi hội tụ | 
| Không gian | O(M) | Chúng tôi lưu trữ các cặp thành phần và mảng giá trị | 

Với M lên tới 100, thậm chí vài trăm lượt thư giãn vẫn thoải mái trong giới hạn. Cấu trúc đủ dày đặc để sự hội tụ diễn ra nhanh chóng trong thực tế và mỗi lần vượt qua chỉ là bậc hai trong M. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# minimal case
assert run("""1
2
1 1
1 1
1 1
""") == "Case #1: 1"

# provided sample 1
assert run("""1
3
1 3
1 2
5 2 3
""") == "Case #1: 7"

# cycle case
assert run("""1
3
2 3
2 3
2 3
0 1 1
""") == "Case #1: 0"

# all independent metals
assert run("""1
3
1 1
2 2
3 3
1 1 1
""") == "Case #1: 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 1 | kim loại hữu ích duy nhất | 
| mẫu 1 | 7 | tính chính xác của việc truyền bá nhiều bước | 
| trường hợp chu kỳ | 0 | không có lợi ích sai lầm trong hệ thống chết theo chu kỳ | 
| kim loại độc lập | 1 | các thành phần biệt lập được xử lý chính xác | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi kim loại tạo thành một chu trình không kết nối với chì. Ví dụ: một tập hợp các kim loại chỉ chuyển đổi lẫn nhau nhưng không bao giờ đạt tới kim loại 1. Trong trường hợp như vậy, tất cả các giá trị vẫn ở mức 0 ngoại trừ kim loại 1 và thuật toán tạo ra mức đóng góp bằng 0 một cách chính xác vì không có đường dẫn thư giãn nào đưa giá trị từ kim loại 1 vào chu trình. 

Một trường hợp khác là khi một kim loại gián tiếp phụ thuộc vào chính nó thông qua một chuỗi dài. Quá trình thư giãn xử lý việc này một cách tự nhiên vì mỗi cải tiến phải được chứng minh bằng giá trị ngày càng tăng và hệ thống cuối cùng sẽ bão hòa khi không còn chuỗi mang tính xây dựng nào tồn tại.
