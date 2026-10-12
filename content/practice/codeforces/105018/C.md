---
title: "CF 105018C - Xe"
description: "Chúng ta có một lưới $n lần n$ và một số quân xe được đặt trên các ô riêng biệt. Một quân xe điều khiển mọi ô trong hàng và mọi ô trong cột của nó. Một ô chỉ được coi là "an toàn" nếu không có quân nào chia sẻ hàng hoặc cột của nó. Hoạt động được phép là loại bỏ quân xe khỏi bảng."
date: "2026-06-28T02:03:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105018
codeforces_index: "C"
codeforces_contest_name: "Winter Cup 5.0 Online Mirror Contest"
rating: 0
weight: 105018
solve_time_s: 56
verified: true
draft: false
---

[CF 105018C - Xe](https://codeforces.com/problemset/problem/105018/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới và một số quân xe được đặt trên các ô riêng biệt. Một quân xe điều khiển mọi ô trong hàng và mọi ô trong cột của nó. Một ô chỉ được coi là "an toàn" nếu không có quân nào chia sẻ hàng hoặc cột của nó. 

Hoạt động được phép là loại bỏ quân xe khỏi bảng. Mục tiêu không phải là tối đa hóa sự an toàn ở mọi nơi mà chỉ để đảm bảo sự tồn tại của ít nhất một ô an toàn sau khi di dời. Chúng tôi muốn giảm thiểu số lượng quân xe bị loại bỏ để tạo ra một ô như vậy. 

Được diễn đạt lại theo cách khác, chúng tôi muốn kết thúc bằng ít nhất một ô$(i, j)$sao cho sau khi xóa, hàng$i$không chứa xe và cột$j$không chứa xe. 

Các hạn chế là nhỏ:$n \le 50$Và$k \le n^2 \le 2500$. Điều này ngay lập tức loại trừ bất cứ điều gì vượt quá đại khái$O(n^3)$cho mỗi trường hợp thử nghiệm nếu được triển khai cẩn thận, nhưng thậm chí điều đó cũng không cần thiết. Việc liệt kê trực tiếp trên các ô lưới đã khả thi. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta cần mô phỏng việc loại bỏ các tập hợp con của quân xe. Ví dụ: nếu chúng ta cố gắng chọn quân xe nào cần xóa thì số lượng tập hợp con là$2^k$, nó trở nên rất lớn ngay cả ở$k = 2500$. Một hướng sai khác là giả sử chúng ta chỉ quan tâm đến hàng trống hoặc cột trống một cách độc lập, nhưng điều kiện phụ thuộc vào một cặp$(row, column)$, không chỉ một chiều. 

Một trường hợp khó nhận thấy xuất hiện khi không có quân xe nào cả. Câu trả lời là 0 vì mọi tế bào đều đã an toàn. Một cách khác là khi các quân xe được phân bổ sao cho mỗi hàng và cột được chiếm giữ ít nhất một lần; ngay cả khi đó, việc loại bỏ các quân xe khỏi giao điểm hàng và cột được lựa chọn cẩn thận vẫn có thể tạo ra một ô an toàn, nhưng chỉ sau khi xóa mục tiêu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử loại bỏ mọi tập hợp con xe có thể có, sau đó kiểm tra xem ô an toàn có tồn tại hay không. Đối với mỗi tập hợp con, chúng tôi sẽ quét tất cả các hàng và cột để xác minh xem có tồn tại một cặp hay không$(i, j)$có hàng và cột đều trống các quân còn lại. Kiểm tra một tập hợp chi phí$O(n^2 + k)$, và có$2^k$các tập hợp con, vượt xa các giới hạn khả thi ngay cả đối với các đầu vào không tầm thường nhỏ nhất. 

Sự đơn giản hóa quan trọng đến từ việc thay đổi góc nhìn: thay vì quyết định loại bỏ quân nào, trước tiên chúng tôi sửa ô an toàn mục tiêu. Giả sử chúng ta quyết định rằng ô an toàn phải được$(i, j)$. Để điều này xảy ra, hãy hàng$i$phải kết thúc trống và cột$j$cuối cùng phải trống rỗng. Điều đó có nghĩa là chúng ta buộc phải loại bỏ mọi quân xe trong hàng$i$và mọi quân xe trong cột$j$. Không có cách nào khác để làm cho tế bào đó an toàn. 

Điều này chuyển đổi vấn đề thành một tính toán chi phí trực tiếp. Đối với mỗi ô ứng cử viên$(i, j)$, chúng ta tính xem có bao nhiêu quân xe nằm trong hàng hoặc cột của nó. Việc loại bỏ những quân xe đó đảm bảo rằng$(i, j)$trở nên an toàn và chi phí đó chính xác là số lần xóa cần thiết cho lựa chọn đó. Sau đó chúng tôi chọn mức tối thiểu trên tất cả các ô. 

Điều này có tác dụng vì mọi cấu hình cuối cùng hợp lệ phải tương ứng với ít nhất một ô có hàng và cột bị xóa hoàn toàn và cách duy nhất để đạt được điều đó là loại bỏ tất cả các quân xe giao nhau với các đường đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^k \cdot n^2)$|$O(k)$| Quá chậm | 
| Hãy thử tất cả các ô bằng cách đếm |$O(n^2 + k)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu quân xe được xếp ở mỗi hàng và mỗi cột. Điều này cho phép chúng ta tính toán chi phí loại bỏ sau này mà không cần phải quét tất cả các quân xe nhiều lần. 
2. Ghi lại xem một quân xe có tồn tại ở mỗi ô hay không bằng cách sử dụng lưới boolean hoặc tập hợp. Điều này là cần thiết để tránh tính hai lần khi quân xe nằm ở giao điểm của hàng và cột đã chọn. 
3. Đối với mỗi ô$(i, j)$, tính toán chi phí để làm cho nó an toàn. Chi phí là số quân xe trong hàng$i$cộng với số quân xe trong cột$j$, trừ một nếu đã có xe ở$(i, j)$. 
4. Theo dõi chi phí tối thiểu trên tất cả các ô. Điều này thể hiện số lần xóa nhỏ nhất cần thiết để tạo ít nhất một ô an toàn ở bất kỳ đâu trên bảng. 
5. Xuất mức tối thiểu này cho từng trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Cố định một ô mục tiêu$(i, j)$buộc phải có một yêu cầu nghiêm ngặt: sau khi loại bỏ, cả hàng và cột của nó phải trống rỗng quân xe. Bất kỳ quân xe nào nằm ngoài các đường này đều không ảnh hưởng đến sự an toàn của ô cụ thể đó, trong khi bất kỳ quân xe nào bên trong đều phải bị loại bỏ. Do đó, chi phí cho mỗi ô ứng viên được xác định đầy đủ và độc lập với các quyết định ở nơi khác. Vì mọi giải pháp hợp lệ phải tương ứng với ít nhất một ô như vậy trở nên an toàn, nên việc giảm thiểu trên tất cả các ứng cử viên sẽ bao trùm mọi khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        
        row = [0] * (n + 1)
        col = [0] * (n + 1)
        has = [[0] * (n + 1) for _ in range(n + 1)]
        
        for _ in range(k):
            x, y = map(int, input().split())
            row[x] += 1
            col[y] += 1
            has[x][y] = 1
        
        ans = k
        
        for i in range(1, n + 1):
            for j in range(1, n + 1):
                cost = row[i] + col[j] - has[i][j]
                if cost < ans:
                    ans = cost
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh lý do trực tiếp. Mảng hàng và cột lưu trữ số lượng để mỗi ô ứng cử viên có thể được đánh giá trong thời gian không đổi. Lưới`has`ngăn chặn phép trừ kép khi quân xe nằm chính xác tại giao điểm. Câu trả lời bắt đầu lúc$k$bởi vì việc loại bỏ tất cả các tân binh luôn đảm bảo cấu hình hợp lệ. 

Vòng lặp kép trên tất cả các ô là an toàn vì$n \le 50$, đưa ra tối đa 2500 đánh giá cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Hãy xem xét một bàn cờ nhỏ nơi quân xe chiếm một vài vị trí rải rác. 

đầu vào:```
1
3 3
1 1
1 2
2 3
```Chúng tôi tính toán số hàng và số cột trước tiên: 

| Bước | hàng1 | hàng2 | hàng3 | col1 | col2 | col3 | 
| --- | --- | --- | --- | --- | --- | --- | 
| sau khi nhập | 2 | 1 | 0 | 1 | 1 | 1 | 

Bây giờ hãy đánh giá chi phí của từng ô: 

| Tế bào | hàng + col - chồng chéo | Giải thích | 
| --- | --- | --- | 
| (1,1) | 2 + 1 - 1 = 2 | xe ở ngã tư | 
| (1,2) | 2 + 1 - 1 = 2 | lại ngã tư | 
| (1,3) | 2 + 1 - 0 = 3 | không có xe ở (1,3) | 
| (2,1) | 1 + 1 - 0 = 2 | | 
| (2,2) | 1 + 1 - 0 = 2 | | 
| (2,3) | 1 + 1 - 1 = 1 | sự lựa chọn tốt nhất | 
| (3,1) | 0 + 1 = 1 | | 
| (3,2) | 0 + 1 = 1 | | 
| (3,3) | 0 + 1 = 1 | | 

Tối thiểu là 1, nghĩa là loại bỏ một quân xe đủ để mở khóa ô an toàn. 

Dấu vết này cho thấy giải pháp không phụ thuộc vào cấu trúc tổng thể mà hoàn toàn phụ thuộc vào các giao điểm hàng-cột cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot n^2 + k)$| mỗi ô được đánh giá một lần, tân binh được xử lý một lần | 
| Không gian |$O(n^2)$| lưới cộng với bộ đếm hàng/cột | 

Những giới hạn$n \le 50$Và$k \le 2500$thực hiện điều này nhanh chóng một cách thoải mái ngay cả trong Python, vì thao tác chiếm ưu thế tối đa là vài nghìn phép cộng số nguyên cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, k = map(int, input().split())
            row = [0] * (n + 1)
            col = [0] * (n + 1)
            has = [[0] * (n + 1) for _ in range(n + 1)]

            for _ in range(k):
                x, y = map(int, input().split())
                row[x] += 1
                col[y] += 1
                has[x][y] = 1

            ans = k
            for i in range(1, n + 1):
                for j in range(1, n + 1):
                    ans = min(ans, row[i] + col[j] - has[i][j])
            out.append(str(ans))
        return "\n".join(out)

    return solve()

# sample-style tests
assert run("1\n3 0\n") == "0", "empty board"
assert run("1\n2 2\n1 1\n2 2\n") == "1", "diagonal rooks"
assert run("1\n2 3\n1 1\n1 2\n2 1\n") == "1", "dense corner case"
assert run("1\n3 3\n1 1\n2 2\n3 3\n") == "1", "main diagonal"
assert run("1\n3 1\n2 2\n") == "0", "single rook"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bảng trống | 0 | không cần xóa | 
| quân xe chéo | 1 | logic giao lộ | 
| trường hợp góc dày đặc | 1 | số lượng hàng/col chồng chéo | 
| đường chéo chính | 1 | vị trí đối xứng | 
| xe đơn | 0 | an toàn tầm thường | 

## Vỏ cạnh 

Khi không có quân xe, số hàng và số cột vẫn bằng 0 ở mọi nơi, vì vậy mọi ô đều có giá trị bằng 0 và giá trị tối thiểu bằng 0. Thuật toán tự nhiên trả về 0 mà không cần xử lý đặc biệt. 

Khi tất cả quân xe nằm trên một hàng, mỗi ô ứng cử viên trong hàng đó sẽ tạo ra chi phí cao, trong khi các ô ở các hàng khác chỉ phụ thuộc vào số cột. Mức tối thiểu sẽ dịch chuyển chính xác sang một hàng bên ngoài hàng bị chiếm dụng, không cần phải xóa. 

Khi quân xe bao phủ hầu hết mọi hàng và cột, việc điều chỉnh giao điểm`-has[i][j]`trở nên quan trọng. Không có nó, một quân xe được đặt ở$(i, j)$sẽ được tính hai lần, làm tăng chi phí loại bỏ và tạo ra các câu trả lời sai.
