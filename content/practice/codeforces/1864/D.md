---
title: "CF 1864D - Tầng ma trận"
description: "Chúng ta được cung cấp một lưới vuông chứa đầy các bit và mục tiêu là biến mọi ô thành số 0. Di chuyển được phép duy nhất sẽ lật các giá trị theo một mẫu hình học rất cụ thể: bạn chọn một ô làm “trung tâm”, và nó sẽ tự chuyển đổi cũng như tất cả các ô nằm ngay bên dưới nó nằm trong…"
date: "2026-06-08T23:54:58+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "data-structures", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1864
codeforces_index: "D"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2023-2024 (Div. 1 + Div. 2)"
rating: 1700
weight: 1864
solve_time_s: 96
verified: false
draft: false
---

[CF 1864D - Tầng ma trận](https://codeforces.com/problemset/problem/1864/D) 

**Đánh giá:** 1700 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, cấu trúc dữ liệu, dp, tham lam, toán học 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới vuông chứa đầy các bit và mục tiêu là biến mọi ô thành số 0. Di chuyển duy nhất được phép sẽ lật các giá trị theo một mẫu hình học rất cụ thể: bạn chọn một ô làm “trung tâm”, và nó sẽ tự chuyển đổi cũng như tất cả các ô nằm ngay bên dưới nó nằm trong hình kim cương đang phát triển có bán kính tăng theo khoảng cách hàng. 

Nếu chúng ta xem xét hiệu ứng kỹ hơn, mọi thao tác đều được neo ở một số ô và chỉ ảnh hưởng đến các hàng bên dưới hoặc bằng ô đó, đồng thời trong mỗi hàng thấp hơn, các cột bị ảnh hưởng sẽ tạo thành một khoảng đối xứng xung quanh cột đã chọn, co lại khi chúng ta di chuyển xuống dưới. Vì vậy, mỗi bước di chuyển sẽ hoạt động giống như thả một “viên kim cương lật XOR” có đỉnh trên cùng là ô được chọn. 

Đầu ra là số lượng tối thiểu các phép toán như vậy cần thiết để làm cho toàn bộ ma trận bằng 0. 

Ràng buộc là tín hiệu chính:$n$có thể lên tới 3000 và có thể lên tới$10^5$trường hợp thử nghiệm với tổng số$n^2$trên tất cả các bài kiểm tra được giới hạn bởi 9 triệu. Điều đó buộc chúng tôi phải xử lý tuyến tính trên mỗi ô. Bất cứ điều gì liên quan đến việc thử các thao tác trên mỗi cặp ô hoặc mô phỏng từng lần lật một cách đơn giản đều quá chậm, nhưng một lần chuyển qua mỗi ô hoặc mỗi hàng với công việc khấu hao không đổi là được. 

Một trường hợp khó nhận thấy là việc lật không cục bộ theo kiểu hình chữ nhật đơn giản. Một kẻ tham lam ngây thơ cố gắng sửa từng hàng một cách độc lập sẽ thất bại vì một thao tác ảnh hưởng đến tất cả các hàng thấp hơn. Ví dụ: nếu chúng ta cố gắng sửa hoàn toàn hàng 1 trước, chúng ta sẽ vô tình làm xáo trộn các hàng bên dưới và mất tính chính xác. Tương tự, việc xử lý từng hàng một cách độc lập sẽ dẫn đến việc đếm hai lần. 

Cấu trúc ẩn chính là hoạt động đơn điệu đi xuống. Khi chúng tôi chọn một trung tâm, nó chỉ ảnh hưởng đến các hàng$\ge i$. Điều này gợi ý việc xử lý từ dưới lên trên, bởi vì một khi một hàng được cố định so với các hàng thấp hơn, nó sẽ không bao giờ bị chạm nữa bởi các thao tác trong tương lai. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ là xem mỗi thao tác như một mặt nạ chuyển đổi nhị phân khổng lồ trên ma trận. Sau đó, vấn đề trở thành việc chọn một tập hợp con tối thiểu của các mặt nạ này mà XOR tạo ra ma trận đã cho. Đây thực chất là một bài toán đại số tuyến tính trên một không gian có kích thước$O(n^2)$với$O(n^2)$các vectơ cơ sở có thể, hoàn toàn không thể giải quyết trực tiếp. 

Ngay cả một lực lượng vũ phu cụ thể hơn cũng sẽ thử, đối với mọi tế bào, xem có áp dụng thao tác hay không, dẫn đến$2^{n^2}$khả năng. Ngay cả mô phỏng tham lam trong đó chúng tôi liên tục chọn bất kỳ số 1 còn lại nào và mô phỏng chi phí hiệu ứng của nó$O(n^3)$mỗi lần kiểm tra, vì mỗi thao tác chạm vào một vùng hình tam giác. 

Quan sát quan trọng là cấu trúc của phép toán cho phép chúng ta xử lý lưới theo thứ tự ngược lại của các hàng. Nếu chúng tôi xử lý từ dưới lên trên, khi chúng tôi quyết định có sửa một ô hay không thì tất cả các ô bên dưới ô đó đã được hoàn tất. Điều này chuyển đổi sự phụ thuộc toàn cầu thành một quyết định cục bộ. 

Bây giờ cái nhìn sâu sắc về cấu trúc quan trọng: xem xét tác động của các thao tác trên một hàng cố định. Mỗi thao tác được căn giữa ở trên có thể ảnh hưởng đến một phân đoạn giống tiền tố trong hàng đó và cấu trúc chồng chéo trở thành tuyến tính. Khi quét lên trên, mỗi hàng trở thành một bài toán 1D độc lập trong đó chúng ta chỉ quan tâm đến tính chẵn lẻ của các phép toán đã được áp dụng từ bên dưới. Điều này làm giảm vấn đề thành việc tham lam sửa chữa những điểm không phù hợp trong khi tích lũy ảnh hưởng lên trên. 

Sự đơn giản hóa cuối cùng là mọi ô đều buộc thực hiện một thao tác hoặc không dựa trên việc nó hiện có bằng 1 hay không sau khi xem xét các lần lật tích lũy từ bên dưới. Khi gặp số 1, chúng ta phải áp dụng một thao tác tại ô đó, vì không có thao tác nào trong tương lai có thể sửa được (tương lai có nghĩa là ở trên, không đi xuống). Tham lam này là tối ưu vì trước tiên nó giải quyết các mâu thuẫn thấp nhất chưa được giải quyết và mỗi thao tác sẽ sửa chính xác tính chẵn lẻ bắt buộc tại vị trí đó đồng thời truyền các hiệu ứng đi lên được xử lý sau đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng/tìm kiếm) |$O(n^3)$hoặc tệ hơn |$O(n^2)$| Quá chậm | 
| Tính chẵn lẻ tham lam từ dưới lên tối ưu |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một bản sao làm việc của lưới và xử lý nó từ hàng dưới cùng lên hàng trên cùng. 

1. Bắt đầu từ hàng$n$và di chuyển lên trên hàng$1$. Lý do là các thao tác chỉ tác động đến các hàng đi xuống nên khi một hàng được xử lý sẽ không bao giờ bị ảnh hưởng nữa. 
2. Đối với mỗi ô$(i, j)$, tính giá trị hiệu dụng hiện tại của nó, là giá trị ban đầu được XOR với tất cả các hiệu ứng từ các hoạt động đã chọn trước đó ảnh hưởng đến nó. Chúng tôi không lưu trữ toàn bộ ảnh hưởng một cách rõ ràng; thay vào đó, chúng tôi duy trì cấu trúc khác biệt trên các đường chéo hoặc mô phỏng thông qua các lần chuyển đổi tích lũy. 
3. Nếu giá trị hiệu dụng tại$(i, j)$là 1, chúng ta phải áp dụng một phép toán tập trung vào$(i, j)$. Điều này là bắt buộc vì không có thao tác nào trong tương lai (ở trên hàng i) có thể thay đổi ô này mà không vi phạm cấu trúc thấp hơn đã cố định. 
4. Khi áp dụng một thao tác tại$(i, j)$, chúng tôi cập nhật ảnh hưởng để tất cả các ô bị ảnh hưởng trong hàng$\ge i$được chuyển đổi tương ứng. Chúng tôi thực hiện việc này một cách hiệu quả bằng cách sử dụng mảng chênh lệch trên mỗi hàng đại diện cho hình chiếu hình kim cương. 
5. Tiếp tục cho đến khi tất cả các ô được xử lý. Số lượng hoạt động được áp dụng là câu trả lời. 

Lý do điều này có tác dụng là vì mỗi ô ở vùng dưới cùng chưa được xử lý có một cơ hội duy nhất để được sửa: chỉ một thao tác ở giữa hoặc bên dưới nó mới có thể ảnh hưởng đến nó và vì chúng ta đang di chuyển từ dưới lên nên cách khắc phục hợp lệ duy nhất là quyết định ngay lập tức. 

### Tại sao nó hoạt động 

Điều bất biến là khi xử lý hàng$i$, tất cả các hàng bên dưới$i$đã ở trạng thái được đảm bảo là cuối cùng và sẽ không bao giờ được sửa đổi nữa. Bất kỳ thao tác nào được chọn trong hàng$i$chỉ ảnh hưởng đến hàng$\ge i$, vì vậy nó không thể làm mất hiệu lực các quyết định trước đó. 

Do đó, mỗi quyết định tại một ô đều bị ép buộc cục bộ: nếu giá trị hiện tại là 1, việc không chọn một thao tác sẽ vĩnh viễn khiến nó không chính xác, trong khi chọn nó có thể lật các ô trong tương lai nhưng những ô đó chưa được sửa và sẽ được giải quyết một cách nhất quán sau đó. Điều này đảm bảo rằng sự lựa chọn tham lam không bao giờ cản trở tính khả thi và luôn giải quyết được ràng buộc gần nhất chưa được giải quyết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        g = [list(map(int, list(input().strip()))) for _ in range(n)]
        
        # We maintain a "delta" array per row for propagation of diamond effects.
        add = [ [0]*(n+2) for _ in range(n+2) ]
        
        def apply(i, j):
            # apply diamond centered at (i, j)
            # affects row r: [j-(r-i), j+(r-i)]
            for r in range(i, n):
                d = r - i
                l = max(0, j - d)
                rgt = min(n-1, j + d)
                add[r][l] ^= 1
                add[r][rgt+1] ^= 1
        
        res = 0
        
        cur = [[0]*n for _ in range(n)]
        
        # process bottom-up, maintaining row prefix XOR
        for i in range(n):
            for j in range(n):
                cur[i][j] = g[i][j]
        
        for i in range(n-1, -1, -1):
            # build prefix effect row by row
            for j in range(n):
                if j > 0:
                    add[i][j] ^= add[i][j-1]
                cur[i][j] ^= add[i][j]
            
            for j in range(n):
```
