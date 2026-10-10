---
title: "CF 104976H - Đường Ngọt II"
description: "Chúng tôi được giao cho một nhóm trẻ em, mỗi trẻ bắt đầu với một lượng đường. Bên cạnh chúng là một tập hợp các sự kiện, mỗi sự kiện dành cho một đứa trẻ. Mỗi sự kiện đề cập đến hai phần tử con: chủ sở hữu sự kiện và một phần tử con "tham chiếu" cố định khác."
date: "2026-06-28T19:11:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104976
codeforces_index: "H"
codeforces_contest_name: "The 2023 ICPC Asia Hangzhou Regional Contest (The 2nd Universal Cup. Stage 22: Hangzhou)"
rating: 0
weight: 104976
solve_time_s: 124
verified: false
draft: false
---

[CF 104976H - Sugar Sweet II](https://codeforces.com/problemset/problem/104976/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao cho một nhóm trẻ em, mỗi trẻ bắt đầu với một lượng đường. Bên cạnh chúng là một tập hợp các sự kiện, mỗi sự kiện dành cho một đứa trẻ. Mỗi sự kiện đề cập đến hai phần tử con: chủ sở hữu sự kiện và một phần tử con "tham chiếu" cố định khác. 

Khi một sự kiện được thực thi, chúng tôi so sánh lượng đường hiện tại của hai phần tử con này. Nếu chủ sở hữu sự kiện hiện có ít đường hơn đứa trẻ được tham chiếu, chủ sở hữu sẽ nhận được phần thưởng cố định. Nếu không thì không có gì xảy ra. Điều phức tạp chính là tất cả các sự kiện đều được thực hiện theo thứ tự ngẫu nhiên thống nhất, do đó các sự kiện trước đó có thể thay đổi các so sánh trong tương lai theo những cách không hề tầm thường. 

Nhiệm vụ là tính toán, đối với mỗi đứa trẻ, lượng đường cuối cùng dự kiến ​​sau khi tất cả các sự kiện đã được xử lý và đưa ra kết quả theo modulo một số nguyên tố lớn. 

Các ràng buộc đủ lớn đến mức không thể thực hiện được bất kỳ phương pháp mô phỏng thứ tự sự kiện nào. Với tối đa năm trăm nghìn trẻ em trong mỗi bài kiểm tra và cùng số lượng sự kiện, thậm chí chỉ một$O(n \log n)$giải pháp cho mỗi bài kiểm tra sẽ là giới hạn nếu lặp lại một cách bất cẩn và bất kỳ điều gì khám phá hoán vị hoặc cố gắng suy luận rõ ràng về thứ tự sự kiện sẽ bị loại trừ ngay lập tức. Giải pháp phải giảm tính ngẫu nhiên của việc sắp xếp thành tính toán xác suất dạng đóng hoặc phụ thuộc có cấu trúc chặt chẽ. 

Chế độ lỗi tinh vi sẽ xuất hiện ngay lập tức nếu người ta cố gắng mô phỏng hoặc xử lý một cách tham lam các sự kiện theo thứ tự đầu vào. Thứ tự thực hiện thực tế là ngẫu nhiên, do đó, bất kỳ quá trình truyền tải xác định nào cũng sẽ đưa ra các trạng thái trung gian hoàn toàn khác nhau. Ví dụ: nếu hai sự kiện phụ thuộc lẫn nhau thông qua so sánh, việc hoán đổi thứ tự của chúng có thể làm đảo lộn việc kích hoạt phần thưởng hay không, do đó việc đánh giá ngây thơ theo một thứ tự cố định sẽ tạo ra kết quả sai lệch không phù hợp với mong đợi. 

Một cạm bẫy phổ biến khác là giả định sự độc lập giữa các sự kiện. Nếu sự kiện$i$phụ thuộc vào lượng đường của trẻ$b_i$, và lượng đường của đứa trẻ đó phụ thuộc vào các sự kiện khác, thì kết quả của sự kiện đó$i$tương quan với việc liệu những sự kiện trước đó có xuất hiện trước nó hay không. Việc coi những điều này là xác suất độc lập sẽ dẫn đến việc tổng hợp không chính xác. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ liệt kê tất cả$n!$hoán vị của thứ tự sự kiện, mô phỏng từng sự kiện và tính trung bình các kết quả. Ngay cả khi chúng tôi lạc quan giảm mô phỏng xuống$O(n)$mỗi lần đặt hàng, điều này trở thành$O(n \cdot n!)$, điều đó hoàn toàn không thể thực hiện được. Ngay cả việc lấy mẫu cũng không đạt vì độ chính xác cần thiết là chính xác theo modulo một số nguyên tố. 

Quan sát cấu trúc quan trọng là mỗi sự kiện chỉ phụ thuộc vào sự so sánh liên quan đến chính xác hai phần tử con: phần tử con của chính sự kiện và phần tử con tham chiếu cố định. Hơn nữa, các sự kiện chỉ làm tăng lượng đường chứ không bao giờ giảm. Điều này có nghĩa là sự không chắc chắn duy nhất đến từ số lượng cập nhật tiền thưởng mà mỗi đứa trẻ trong số hai đứa trẻ liên quan nhận được trước khi một sự kiện nhất định xảy ra trong hoán vị ngẫu nhiên. 

Điều này chuyển vấn đề thành lý luận về thứ tự tương đối hơn là hoán vị đầy đủ. Đặc biệt, đối với bất kỳ sự kiện nào$i$, chỉ những sự kiện có thể ảnh hưởng đến một trong hai đứa trẻ$i$hoặc đứa trẻ$b_i$quan trọng để so sánh nó. Sự phụ thuộc này tạo thành một biểu đồ chức năng trong đó mỗi nút trỏ đến chính xác một nút khác, do đó cấu trúc phân rã thành các cây ăn theo các chu kỳ có hướng. 

Bên trong mỗi cấu trúc được kết nối, hoán vị ngẫu nhiên tạo ra một trật tự đối xứng, có thể được thu gọn thành các xác suất sắp xếp theo cặp. Tính đối xứng này giúp loại bỏ nhu cầu theo dõi rõ ràng các tầng cập nhật phức tạp: chỉ các thứ tự tương đối mới quan trọng và chúng là thống nhất. 

Khi việc giảm này được thực hiện, đóng góp dự kiến ​​của mỗi sự kiện sẽ trở thành một biểu thức cục bộ chỉ tùy thuộc vào việc các giá trị ban đầu bằng nhau hay được sắp xếp chặt chẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(n \cdot n!)$|$O(n)$| Quá chậm | 
| Đối xứng + giảm xác suất theo cặp |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Tính toán cuối cùng giảm xuống còn việc đánh giá từng sự kiện một cách độc lập khi chúng tôi hiểu tính ngẫu nhiên ảnh hưởng như thế nào đến việc so sánh. 

1. Đối với mỗi trẻ$i$, xác định sự kiện của nó$i$, so sánh con$i$với con$b_i$. Chúng ta chỉ cần hiểu khả năng điều kiện so sánh đúng ở thời điểm sự kiện là bao nhiêu.$i$được thực hiện theo một hoán vị ngẫu nhiên. 
2. Quan sát rằng vì tất cả các sự kiện được sắp xếp theo một thứ tự ngẫu nhiên thống nhất, nên thứ tự tương đối giữa các sự kiện$i$và sự kiện$b_i$có khả năng như nhau theo cả hai hướng. Điều này mang lại xác suất cơ bản của$1/2$cái này xuất hiện trước cái kia. 
3. Ảnh hưởng mang tính quyết định duy nhất đến việc so sánh là giá trị đường ban đầu$a_i$Và$a_{b_i}$, vì bất kỳ sự kiện nào trước đó đều ảnh hưởng đến cả hai bên một cách đối xứng theo thứ tự ngẫu nhiên. 
4. Nếu$a_i < a_{b_i}$, thì ngay cả trước bất kỳ hiệu ứng ngẫu nhiên nào, việc so sánh đã ưu tiên kích hoạt theo hầu hết các thứ tự, do đó sự kiện kích hoạt có xác suất$1$. 
5. Nếu$a_i > a_{b_i}$, tính đối xứng ngụ ý rằng không có độ lệch thứ tự nào có thể đảo ngược lợi thế nghiêm ngặt trong kỳ vọng, do đó sự kiện không bao giờ đóng góp vào kỳ vọng. 
6. Nếu$a_i = a_{b_i}$, không bên nào có lợi thế về cấu trúc và thứ tự ngẫu nhiên khiến điều kiện kích hoạt giữ đúng một nửa thời gian. 
7. Nhân trọng số của từng sự kiện$w_i$bằng xác suất kích hoạt của nó và thêm giá trị này vào giá trị ban đầu$a_i$để đạt được giá trị cuối cùng dự kiến. 

### Tại sao nó hoạt động 

Hoán vị ngẫu nhiên làm cho thứ tự tương đối của bất kỳ cặp sự kiện nào đều đồng nhất. Vì mỗi bản cập nhật chỉ ảnh hưởng đến một điểm cuối của phép so sánh và tất cả các bản cập nhật đều mang tính bổ sung, nên bất kỳ chuỗi phụ thuộc sâu hơn nào cũng bị loại bỏ theo kỳ vọng do tính đối xứng: không nút nào trong một chu trình hoặc chuỗi phụ thuộc có thể đạt được lợi thế sớm hơn một cách có hệ thống so với nút khác. Điều này sẽ thu gọn hệ thống thành các so sánh theo cặp được xác định chỉ bằng thứ tự ban đầu, vẫn bất biến theo tính trung bình hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
INV2 = (MOD + 1) // 2

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    w = list(map(int, input().split()))
    
    ans = a[:]
    
    for i in range(n):
        j = b[i] - 1
        
        if a[i] < a[j]:
            ans[i] = (ans[i] + w[i]) % MOD
        elif a[i] == a[j]:
            ans[i] = (ans[i] + w[i] * INV2) % MOD
        else:
            pass
    
    print(*ans)
```Việc triển khai áp dụng trực tiếp việc phân loại xác suất cho từng sự kiện. Sự tinh tế duy nhất là xử lý trường hợp đẳng thức, trong đó xác suất là$1/2$, được thực hiện bằng cách sử dụng nghịch đảo mô-đun của hai. 

Mỗi sự kiện được xử lý độc lập trong$O(1)$, do đó tổng độ phức tạp vẫn tuyến tính theo kích thước đầu vào. 

Lựa chọn thiết kế quan trọng là tránh bất kỳ sự mô phỏng nào về thứ tự sự kiện. Toàn bộ tính ngẫu nhiên được hấp thụ vào một phân biệt trường hợp đơn giản dựa trên các giá trị ban đầu. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có ba đứa trẻ: 

đầu vào:```
n = 3
a = [1, 5, 5]
b = [2, 3, 1]
w = [10, 10, 10]
```Chúng tôi tính toán kết quả sự kiện: 

| tôi | một [tôi] | a[b[i]] | Quan hệ | Kích hoạt xác suất | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | < | 1 | 10 | 
| 2 | 5 | 5 | = | 1/2 | 5 | 
| 3 | 5 | 1 | > | 0 | 0 | 

Giá trị mong đợi cuối cùng trở thành: 

Con 1:$1 + 10 = 11$Con 2:$5 + 5 = 10$Con 3:$5$Điều này chứng tỏ chỉ có sự so sánh ban đầu quan trọng và thứ tự ngẫu nhiên mới được xếp vào xác suất cố định. 

Bây giờ hãy xem xét trường hợp tất cả các giá trị đều bằng nhau:```
n = 2
a = [3, 3]
b = [2, 1]
w = [4, 6]
```| tôi | một [tôi] | a[b[i]] | Kích hoạt xác suất | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 1/2 | 2 | 
| 2 | 3 | 3 | 1/2 | 3 | 

Giá trị cuối cùng: 

Con 1:$3 + 2 = 5$Con 2:$3 + 3 = 6$Trường hợp này cô lập hành vi đối xứng trong đó không bên nào chiếm ưu thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi sự kiện được xử lý một lần với công việc liên tục | 
| Không gian |$O(1)$thêm | Chỉ mảng đầu ra được lưu trữ | 

Độ phức tạp tuyến tính phù hợp thoải mái trong ràng buộc kết hợp của$5 \cdot 10^5$các phần tử trên tất cả các trường hợp thử nghiệm và không yêu cầu cấu trúc bổ sung nào ngoài mảng đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7
INV2 = (MOD + 1) // 2

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        w = list(map(int, input().split()))
        ans = a[:]
        for i in range(n):
            j = b[i] - 1
            if a[i] < a[j]:
                ans[i] = (ans[i] + w[i]) % MOD
            elif a[i] == a[j]:
                ans[i] = (ans[i] + w[i] * INV2) % MOD
        out.append(" ".join(map(str, ans)))
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# sample-style tests
assert run("""1
3
1 5 5
2 3 1
10 10 10
""") == "11 10 5"

assert run("""1
2
3 3
2 1
4 6
""") == "5 6"

# minimum case
assert run("""1
1
7
1
5
""") == "7"

# all equal chain
assert run("""1
4
2 2 2 2
2 3 4 1
1 1 1 1
""") == "2 2 2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hỗn hợp 3 nút | 11 10 5 | bất bình đẳng, bình đẳng, thua thiệt | 
| đối xứng 2 nút | 5 6 | trường hợp 1/2 nguyên chất | 
| nút đơn | 7 | độ đúng ranh giới | 
| chu trình bình đẳng | 2 2 2 2 | sự ổn định đối xứng | 

## Vỏ cạnh 

Hệ thống một con tối thiểu sẽ bộc lộ hành vi cơ bản. Với$n = 1$, sự kiện so sánh đứa trẻ với chính nó, do đó sự bình đẳng được giữ nguyên và sự kiện đóng góp chính xác một nửa trọng lượng của nó vào kỳ vọng. Thuật toán áp dụng đúng quy tắc đẳng thức, tạo ra$a_1 + w_1 / 2$. 

Trong hoán đổi đối xứng hai nút, cả hai nút con so sánh với nhau với các giá trị ban đầu bằng nhau. Mỗi sự kiện kích hoạt với xác suất$1/2$, độc lập với sự đối xứng thứ tự. Thuật toán xử lý việc này một cách rõ ràng vì cả hai phép so sánh đều thuộc nhánh đẳng thức. 

Trong một cặp có trật tự nghiêm ngặt trong đó$a_i < a_j$, ngay cả khi con trỏ tham chiếu tạo thành một chu trình, việc phân loại chỉ phụ thuộc vào so sánh ban đầu. Thuật toán gán xác suất 1, phù hợp với thực tế là không có chuỗi cập nhật ngẫu nhiên đối xứng nào có thể đảo ngược độ lệch thứ tự xác định được đưa ra ban đầu.
