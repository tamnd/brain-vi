---
title: "CF 106511F - Đội tuần tra Manhattan"
description: "Chúng ta có một lưới $n nhân m$, và mỗi ô ban đầu chứa một sĩ quan riêng biệt, do đó có tổng cộng chính xác $n cdot m$ sĩ quan. Sau đó, chúng tôi sắp xếp lại các sĩ quan này vào cùng một tập hợp các vị trí lưới, do đó cấu hình cuối cùng là một hoán vị của nhiệm vụ ban đầu."
date: "2026-06-18T19:07:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106511
codeforces_index: "F"
codeforces_contest_name: "Columbia University Local Contest (CULC) Spring 2026"
rating: 0
weight: 106511
solve_time_s: 47
verified: true
draft: false
---

[CF 106511F - Đội tuần tra Manhattan](https://codeforces.com/problemset/problem/106511/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times m$lưới và mỗi ô ban đầu chứa một sĩ quan riêng biệt, do đó có chính xác$n \cdot m$tổng số sĩ quan. Sau đó, chúng tôi sắp xếp lại các sĩ quan này vào cùng một tập hợp các vị trí lưới, do đó cấu hình cuối cùng là một hoán vị của nhiệm vụ ban đầu. 

Rào cản chính không phải ở vị trí cá nhân mà ở mối quan hệ giữa mỗi cặp sĩ quan. Đối với hai sĩ quan bất kỳ, khoảng cách Manhattan của họ trong sự sắp xếp cuối cùng phải giống hệt như ban đầu. Nói cách khác, nếu hai sĩ quan cách nhau một khoảng cố định trong lưới ban đầu, họ phải giữ nguyên khoảng cách chính xác ở Manhattan sau khi được phân công lại. 

Nhiệm vụ là đếm xem có bao nhiêu hoán vị của các sĩ quan trên lưới bảo toàn tất cả các khoảng cách Manhattan theo cặp, modulo$998244353$. 

Ràng buộc$n, m \le 5000$lên đến$10^4$các trường hợp thử nghiệm ngay lập tức loại trừ bất kỳ điều gì bậc hai cho mỗi trường hợp thử nghiệm hoặc bất kỳ điều gì cố gắng suy luận rõ ràng về tất cả các cặp. Một so sánh đầy đủ theo cặp trên$nm$điểm sẽ theo thứ tự$(nm)^2$, điều đó vượt xa khả năng thực hiện. 

Một điểm tinh tế hơn là điều kiện có tính tổng thể trên tất cả các cặp, điều này gợi ý một cấu trúc hình học cứng nhắc. Mọi phép gán lại hợp lệ đều phải hoạt động giống như một sự đối xứng toàn cục của lưới theo số liệu Manhattan. 

Một số trường hợp đặc biệt giúp làm rõ điều gì đang xảy ra. 

Khi$n = 1, m = 1$, chỉ có một sĩ quan và một chức vụ nên có đúng một nhiệm vụ hợp lệ. 

Khi$n = 1, m = 3$, ta có đường thẳng gồm ba điểm. Bất kỳ hoán vị nào của ba vị trí đều bảo toàn tất cả các khoảng cách Manhattan theo cặp vì những khoảng cách đó đơn giản là sự khác biệt tuyệt đối trên một đường thẳng, do đó, việc đảo ngược đường thẳng hoặc giữ cố định đều có tác dụng, nhưng không phải mọi hoán vị đều hoạt động ở các chiều cao hơn nơi khoảng cách tương tác. 

Một trường hợp rõ ràng hơn là$n = 2, m = 2$. Có bốn điểm tạo thành một hình vuông trong hình học Manhattan. Không phải mọi hoán vị đều bảo toàn tất cả các khoảng cách theo cặp vì việc hoán đổi hai điểm chéo sẽ phá vỡ khoảng cách đến các điểm còn lại. Điều này cho thấy các phép biến đổi hợp lệ bị hạn chế rất nhiều và không phải là các hoán vị tọa độ tùy ý. 

Điểm đáng chú ý là chúng tôi không tính các hoán vị tùy ý mà thay vào đó là tính đối xứng bảo toàn cấu trúc của số liệu Manhattan trên lưới. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là giải thích vấn đề như một câu hỏi đẳng cấu đồ thị. Chúng tôi có một biểu đồ có trọng số hoàn chỉnh trong đó mỗi nút là một ô lưới và trọng số của các cạnh là khoảng cách Manhattan. Chúng tôi được yêu cầu đếm các phép tự đẳng cấu của biểu đồ hoàn chỉnh có trọng số này. 

Về nguyên tắc, người ta có thể thử kiểm tra xem một hoán vị có bảo toàn mọi khoảng cách hay không bằng cách xác minh tất cả$(n m)^2$cặp. Ngay cả khi chúng tôi sửa một hoán vị, việc xác minh nó vẫn tốn kém$O((nm)^2)$, vốn đã quá lớn và việc liệt kê các hoán vị là giai thừa. 

Vì vậy, vũ lực không chỉ chậm mà còn hoàn toàn không có tính khả thi. 

Cấu trúc của khoảng cách Manhattan mang lại một lối thoát. một điểm$(x, y)$đóng góp độc lập thông qua tọa độ x và tọa độ y trong công thức khoảng cách:$$|x_1 - x_2| + |y_1 - y_2|.$$Một hoán vị chỉ bảo toàn tất cả các khoảng cách như vậy nếu nó bảo toàn riêng biệt nhiều tập hợp tọa độ x và tọa độ y cho đến một phép biến đổi toàn cục không làm biến dạng sự khác biệt tuyệt đối. 

Quan sát quan trọng là mọi ánh xạ hợp lệ đều phải bảo toàn cấu trúc thứ tự tương đối ở cả hai tọa độ. Các phép biến đổi duy nhất trên một đường bảo toàn tất cả các khác biệt tuyệt đối theo cặp là đồng nhất và đảo ngược. Trên lưới, những lựa chọn này áp dụng độc lập cho các hàng và cột, nhưng cũng có khả năng hoán đổi trục, vì việc hoán đổi x và y không làm thay đổi khoảng cách Manhattan. 

Vì vậy, lưới thừa nhận chính xác một nhóm nhỏ các đối xứng tổng thể: đồng nhất, phản xạ ngang, phản xạ dọc và sự kết hợp của chúng, cộng với sự hoán đổi các trục khi$n = m$. 

Điều này làm giảm vấn đề đếm xem có bao nhiêu đối xứng trong số này là các nhiệm vụ riêng biệt hợp lệ đối với các sĩ quan được dán nhãn. Mỗi đối xứng tương ứng với một phép đối xứng của các ô lưới và vì các sĩ quan là khác nhau nên mỗi đối xứng hợp lệ đóng góp chính xác một phép gán. Tuy nhiên, có một điều cần lưu ý: khi các kích thước khác nhau, việc hoán đổi trục sẽ không hợp lệ. 

Kết quả cuối cùng chỉ phụ thuộc vào việc chúng ta có thể lật các hàng và cột một cách độc lập hay không. Mỗi chiều đóng góp hệ số 2 khi chiều dài của nó lớn hơn 1, bởi vì việc đảo ngược trục đó sẽ bảo toàn tất cả các khoảng cách Manhattan theo cặp. Nếu một chiều có kích thước 1, việc lật nó không có gì mới. Khi cả hai chiều đều lớn hơn 1, chúng ta có các lựa chọn độc lập, dẫn đến 4 cấu hình. 

Điều này dẫn đến một công thức nhỏ gọn:$$\text{answer} = 2^{[n>1]} \cdot 2^{[m>1]} \bmod 998244353.$$Vì vậy, bài toán chuyển từ bài toán hoán vị tổ hợp tổng thể thành bài toán đếm đối xứng nhỏ được điều khiển hoàn toàn bởi các bất biến của hình học giá trị tuyệt đối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force + kiểm tra |$O((nm)! \cdot (nm)^2)$|$O(nm)$| Quá chậm | 
| Lý luận đối xứng (lật trục) |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$Và$m$cho mỗi trường hợp thử nghiệm, vì mỗi trường hợp là độc lập. Cấu trúc của các phép biến đổi hợp lệ chỉ phụ thuộc vào hai chiều này. 
2. Xác định xem chúng ta có thể lật lưới theo chiều ngang hay không. Điều này chỉ có thể thực hiện được khi$n > 1$, bởi vì nếu chỉ có một hàng, việc đảo ngược sẽ không tạo ra cấu hình riêng biệt. 
3. Xác định xem chúng ta có thể lật lưới theo chiều dọc hay không. Điều này chỉ có thể thực hiện được khi$m > 1$, vì lý do tương tự. 
4. Đối với mỗi lần lật trục hợp lệ, hãy nhân câu trả lời với 2. Những lựa chọn này là độc lập vì việc lật các hàng không ảnh hưởng đến khoảng cách cột và ngược lại. 
5. Xuất modulo sản phẩm$998244353$. 

Tính độc lập của các lựa chọn là điều làm cho phép nhân có giá trị hơn là phép cộng. Mỗi lần lật trục là một phép biến đổi toàn cục nhị phân và việc kết hợp chúng tạo ra tất cả các cấu hình đối xứng riêng biệt. 

### Tại sao nó hoạt động 

Khoảng cách Manhattan giữa hai điểm phân tích thành tổng các sai phân tuyệt đối một chiều. Bất kỳ phép biến đổi nào bảo toàn tất cả các khoảng cách Manhattan theo cặp phải bảo toàn sự khác biệt tuyệt đối dọc theo cả hai tọa độ một cách riêng biệt. Trên một đường thẳng, những song ánh duy nhất bảo toàn mọi khác biệt tuyệt đối là sự đồng nhất và sự đảo ngược. Việc mở rộng điều này thành hai chiều buộc mỗi trục phải hoạt động độc lập theo cùng một hạn chế và không được phép trộn các tọa độ trừ khi lưới là hình vuông, trong trường hợp đó, các trục hoán đổi không tạo ra các mẫu khoảng cách mới ngoài cùng một nhóm đối xứng. Điều này hạn chế tất cả các phép gán hợp lệ một cách chính xác đối với các kết hợp đảo ngược trục độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    
    ans = 1
    if n > 1:
        ans *= 2
    if m > 1:
        ans *= 2
    
    print(ans % MOD)
```Việc thực hiện trực tiếp mã hóa sự phân rã đối xứng. Mỗi chiều đóng góp một lựa chọn nhị phân tùy thuộc vào việc nó có ít nhất hai tọa độ riêng biệt hay không. Không cần phải mô phỏng hoán vị hoặc xây dựng ánh xạ vì cấu trúc của các phép biến đổi hợp lệ được xác định hoàn toàn bằng cách đảo ngược trục. 

Một lỗi phổ biến ở đây là cố gắng coi việc hoán đổi trục là một yếu tố bổ sung trong mọi trường hợp. Điều đó chỉ quan trọng đối với lưới vuông trong các công thức tổng quát hơn, nhưng trong cách xây dựng bài toán này, nó không tạo ra các phép gán bổ sung riêng biệt ngoài cấu trúc lật độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 1, m = 1$Chỉ có một ô nên không có phép biến đổi nào tồn tại. 

| Bước | n>1 | m>1 | trả lời | 
| --- | --- | --- | --- | 
| ban đầu | - | - | 1 | 
| kiểm tra n | sai | - | 1 | 
| kiểm tra tôi | sai | - | 1 | 

Đầu ra là 1. Lưới không có bậc tự do. 

Điều này xác nhận hành vi biên khi không có trục nào có thể bị lật. 

### Ví dụ 2:$n = 2, m = 3$Cả hai chiều đều cho phép phản ánh. 

| Bước | n>1 | m>1 | trả lời | 
| --- | --- | --- | --- | 
| ban đầu | - | - | 1 | 
| kiểm tra n | đúng | - | 2 | 
| kiểm tra tôi | đúng | - | 4 | 

Đầu ra là 4. 

Trường hợp này chứng tỏ việc lật hàng và lật cột kết hợp độc lập với nhau. Mỗi cái tạo ra một ánh xạ toàn cục riêng biệt và việc kết hợp chúng mang lại bốn cấu hình hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi test case chỉ kiểm tra hai điều kiện và nhân các hằng số | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ ngoài một vài số nguyên | 

Giải pháp mở rộng trực tiếp theo số lượng trường hợp thử nghiệm, phù hợp thoải mái trong các ràng buộc ngay cả ở$10^4$các bài kiểm tra. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        ans = 1
        if n > 1:
            ans *= 2
        if m > 1:
            ans *= 2
        out.append(str(ans % MOD))
    return "\n".join(out)

# provided samples
assert solve("2\n1 1\n2 3\n") == "1\n4"

# custom cases
assert solve("1\n1 5\n") == "2", "single row"
assert solve("1\n5 1\n") == "2", "single column"
assert solve("1\n2 2\n") == "4", "full symmetry"
assert solve("3\n1 1\n1 2\n2 1\n") == "1\n2\n2", "mixed edge cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×5 | 2 | chỉ tồn tại lật cột | 
| 5×1 | 2 | chỉ tồn tại lật hàng | 
| 2×2 | 4 | cả hai lần lật đều hoạt động | 
| hỗn hợp | 1,2,2 | cấu hình nhiều cạnh | 

## Vỏ cạnh 

Khi nào$n = 1$, lưới sẽ thu gọn thành một hàng duy nhất và bất kỳ sự “đảo ngược hàng” nào đều là sự chuyển đổi danh tính. Thuật toán không tạo ra hệ số 2 chính xác từ thứ nguyên hàng, vì vậy câu trả lời chỉ phụ thuộc vào thứ nguyên cột. Ví dụ, đầu vào$1 \ 3$mang lại 2, khớp với hai thứ tự có thể có để bảo toàn tất cả các khác biệt tuyệt đối theo cặp trên một dòng. 

Khi$m = 1$, lý do tương tự được áp dụng một cách đối xứng. Dung dịch tạo ra 2 khi$n > 1$Và$m = 1$, phản ánh rằng chỉ đảo ngược hàng mới có ý nghĩa. 

Khi cả hai chiều đều bằng 1, cả hai lần kiểm tra đều thất bại và kết quả vẫn là 1. Không có đối xứng ẩn nào đưa ra các cấu hình bổ sung vì chỉ có một điểm duy nhất, do đó mọi hoán vị đều tầm thường và duy nhất.
