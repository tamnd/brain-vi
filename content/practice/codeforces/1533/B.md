---
title: "CF 1533B - Hàm điểm gần nhất"
description: "Chúng ta có một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một mảng các số nguyên riêng biệt được sắp xếp. Hãy coi những con số này như những điểm cố định trên một trục số. Một điểm truy vấn $y$ được chọn và một hàm trả về điểm gần nhất trong mảng về $y$ dựa trên khoảng cách tuyệt đối."
date: "2026-06-14T18:31:16+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1533
codeforces_index: "B"
codeforces_contest_name: "Kotlin Heroes: Episode 7"
rating: 0
weight: 1533
solve_time_s: 252
verified: true
draft: false
---

[CF 1533B - Hàm điểm gần nhất](https://codeforces.com/problemset/problem/1533/B) 

**Đánh giá:** - 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 4 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một mảng các số nguyên riêng biệt được sắp xếp. Hãy coi những con số này như những điểm cố định trên một trục số. Một điểm truy vấn$y$được chọn và hàm trả về điểm gần nhất trong mảng tới$y$dựa trên khoảng cách tuyệt đối. 

Hàm hoạt động bình thường khi có một điểm gần nhất duy nhất. Nó chỉ thất bại khi$y$có khoảng cách chính xác như nhau so với hai giá trị mảng khác nhau, bởi vì khi đó có hai câu trả lời hợp lệ và điểm hòa không được xác định. 

Vì vậy nhiệm vụ không phải là mô phỏng các truy vấn. Thay vào đó, chúng ta phải xác định xem có tồn tại số nguyên nào không$y$điều đó sẽ hạ cánh chính xác ở “vị trí ràng buộc” giữa hai phần tử mảng. 

Kích thước đầu vào lớn: tổng số phần tử trong tất cả các trường hợp thử nghiệm đạt tới$2 \cdot 10^5$, loại trừ bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào thử tất cả các điểm truy vấn ứng cử viên hoặc kiểm tra tất cả các vị trí số nguyên giữa các giá trị sẽ quá chậm vì khoảng cách giữa các số có thể lên tới$10^9$, làm cho việc liệt kê không thể thực hiện được. 

Một sai lầm phổ biến là cho rằng các ràng buộc có thể liên quan đến các phần tử không liền kề. Ví dụ, người ta có thể nghĩ rằng một điểm có thể gần bằng nhau$x_i$Và$x_j$ngay cả khi có các giá trị ở giữa. Điều đó không thể xảy ra, bởi vì bất kỳ điểm trung gian nào cũng sẽ luôn gần với một trong các giá trị bên trong hơn. Một sai lầm tinh tế khác là quên rằng$y$phải là số nguyên. Điểm giữa không phải là số nguyên sẽ không tạo ra sự cố hợp lệ ngay cả khi nó được căn giữa về mặt hình học. 

## Phương pháp tiếp cận 

Ý tưởng brute-force sẽ là lặp lại tất cả các giá trị số nguyên giữa các phần tử mảng tối thiểu và tối đa và, với mỗi phần tử$y$, tính điểm mảng gần nhất. Nếu hai điểm bằng nhau, chúng tôi tuyên bố thành công. Điều này đúng về mặt logic nhưng hoàn toàn không khả thi. Phạm vi giá trị có thể đạt tới$10^9$, vì vậy ngay cả một trường hợp thử nghiệm cũng có thể yêu cầu hàng tỷ lần kiểm tra. 

Nhận xét quan trọng là tỷ số hòa chỉ có thể xảy ra trong một tình huống có tổ chức chặt chẽ. Với hai phần tử liền kề bất kỳ$x_i$Và$x_{i+1}$, tập hợp các điểm gần hơn$x_i$nằm ở phía bên trái của điểm giữa của chúng và tập hợp gần hơn$x_{i+1}$nằm ở phía bên phải. Một trận hòa xảy ra chính xác ở điểm giữa. 

Vì chúng ta chỉ quan tâm đến số nguyên$y$, điểm giữa này phải là một số nguyên, điều này chỉ xảy ra khi$x_i + x_{i+1}$là chẵn. Điều này làm giảm toàn bộ vấn đề để kiểm tra xem có cặp liền kề nào có chênh lệch chẵn hay không. 

Điều này biến đổi những gì trông giống như tìm kiếm hình học thành một lần quét đơn giản về những khác biệt liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả$y$|$O(\text{range} \cdot n)$|$O(1)$| Quá chậm | 
| Kiểm tra tính chẵn lẻ liền kề |$O(n)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giảm vấn đề xuống việc kiểm tra các cặp liền kề trong mảng đã được sắp xếp. 

1. Đọc mảng điểm đã được sắp xếp và không chứa điểm trùng lặp. Thứ tự này đảm bảo rằng bất kỳ sự ràng buộc ứng cử viên nào cũng phải xảy ra giữa những người hàng xóm. 
2. Đối với mỗi cặp liền kề$(x_i, x_{i+1})$, tính sự khác biệt$d = x_{i+1} - x_i$. 
3. Nếu$d$là số chẵn nên biết ngay tồn tại một số nguyên trung điểm$y = \frac{x_i + x_{i+1}}{2}$. Ở giá trị này, cả hai điểm cuối đều có khoảng cách như nhau, điều này đảm bảo xảy ra sự cố. 
4. Nếu không có cặp liền kề nào có hiệu chẵn, hãy kết luận rằng không có điểm giữa số nguyên nào tồn tại ở bất kỳ đâu trong mảng, do đó không thể có điểm bằng nhau. 
5. Lặp lại quy trình này cho tất cả các trường hợp thử nghiệm một cách độc lập. 

Điểm quyết định quan trọng là bước 3, vì nó kết nối trực tiếp cấu trúc số học với tính đối xứng hình học trên trục số. 

### Tại sao nó hoạt động 

Bất kỳ điểm nào$y$gần nhất với một vùng liên tục của trục số được giới hạn bởi các đường phân giác vuông góc giữa các phần tử liên tiếp. Những ranh giới này xảy ra chính xác tại điểm giữa của các cặp liền kề. Vì mảng được sắp xếp nên mọi vùng mơ hồ chỉ được xác định bởi các lân cận và không có cặp nào xa hơn có thể cạnh tranh mà không bị chi phối bởi các điểm trung gian. Do đó, sự tồn tại của một điểm va chạm hợp lệ tương đương với sự tồn tại của một điểm giữa nguyên giữa một số cặp liền kề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    x = list(map(int, input().split()))
    
    ok = False
    for i in range(n - 1):
        if (x[i + 1] - x[i]) % 2 == 0:
            ok = True
            break
    
    print("YES" if ok else "NO")
```Việc thực hiện trực tiếp tuân theo quan sát rằng chỉ những khác biệt liền kề mới quan trọng. Vòng lặp dừng sớm sau khi tìm thấy một cặp hợp lệ, điều này rất quan trọng đối với hiệu quả khi các mảng lớn chứa khoảng cách chẵn sớm. 

Một chi tiết tinh tế là chúng tôi tính toán sự khác biệt chứ không phải tổng. Cả hai đều tương đương với tính chẵn lẻ, nhưng phép trừ tránh mọi lo ngại về các giá trị trung gian lớn, mặc dù Python xử lý các số nguyên lớn một cách an toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
6
1 2 5 7 9 11
```| tôi | x[i] | x[i+1] | sự khác biệt | thậm chí? | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 1 | không | tiếp tục | 
| 1 | 2 | 5 | 3 | không | tiếp tục | 
| 2 | 5 | 7 | 2 | vâng | có thể xảy ra sự cố | 

Tại chỉ số 2, điểm giữa là 6, cách đều 5 và 7 nên hàm này không thành công. 

Bây giờ hãy xem xét:```
6
1 2 5 8 9 12
```| tôi | x[i] | x[i+1] | sự khác biệt | thậm chí? | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 1 | không | tiếp tục | 
| 1 | 2 | 5 | 3 | không | tiếp tục | 
| 2 | 5 | 8 | 3 | không | tiếp tục | 
| 3 | 8 | 9 | 1 | không | tiếp tục | 
| 4 | 9 | 12 | 3 | không | tiếp tục | 

Không có cặp liền kề nào có khoảng cách chẵn, do đó không tồn tại điểm giữa nguyên nào tạo ra sự mơ hồ. Hàm không bao giờ có hai ứng cử viên gần giống nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi mảng được quét một lần để kiểm tra sự khác biệt liền kề | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung ngoài bộ nhớ đầu vào | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó việc quét tuyến tính cho mỗi trường hợp kiểm thử vẫn được thực hiện thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        x = list(map(int, input().split()))
        ok = False
        for i in range(n - 1):
            if (x[i + 1] - x[i]) % 2 == 0:
                ok = True
                break
        out.append("YES" if ok else "NO")
    return "\n".join(out)

# provided samples
assert run("""7
2
1 3
2
1 100
3
1 50 101
2
1 1000000000
2
1 999999999
6
1 2 5 7 9 11
6
1 2 5 8 9 12
""") == """YES
NO
NO
NO
YES
YES
NO"""

# custom cases
assert run("""1
2
10 14
""") == "YES", "even gap midpoint exists"

assert run("""1
2
10 13
""") == "NO", "odd gap no integer midpoint"

assert run("""1
3
1 4 7
""") == "NO", "all gaps odd"

assert run("""1
3
1 4 6
""") == "YES", "one even gap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 14 | CÓ | phát hiện khoảng cách chẵn cơ bản | 
| 10 13 | KHÔNG | khoảng cách lẻ bị từ chối một cách chính xác | 
| 1 4 7 | KHÔNG | nhiều khoảng trống lẻ | 
| 1 4 6 | CÓ | khoảng trống hỗn hợp, một khoảng trống hợp lệ là đủ | 

## Vỏ cạnh 

Đối với mảng có kích thước hai, toàn bộ quyết định sẽ chuyển sang kiểm tra tính chẵn lẻ duy nhất. Nếu hai giá trị khác nhau một số lẻ thì không có số nguyên nào nằm chính xác ở giữa, do đó không thể xảy ra sự cố. Nếu hiệu là chẵn thì điểm giữa là số nguyên và luôn gây ra sự mơ hồ. 

Đối với các mảng thưa thớt lớn, logic tương tự vẫn được áp dụng vì các điểm trung gian không ảnh hưởng đến ranh giới Voronoi dựa trên kề. Ngay cả khi các giá trị cách xa nhau thì chỉ tính chẵn lẻ của khoảng cách mới quan trọng chứ không phải độ lớn. 

Đối với các chuỗi có khoảng cách đều nhau như cấp số cộng, câu trả lời phụ thuộc hoàn toàn vào kích thước bước. Nếu bước chẵn, mọi cặp liền kề sẽ tạo ra một điểm giữa hợp lệ, tạo ra sự cố. Nếu bước này là số lẻ thì không có điểm giữa nào là số nguyên và hàm vẫn an toàn xuyên suốt.
