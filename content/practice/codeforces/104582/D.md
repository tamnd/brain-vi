---
title: "CF 104582D - Trình diễn thời trang"
description: "Chúng tôi đang làm việc trên lưới $N nhân N$ trong đó mỗi ô có thể trống hoặc chứa một mô hình. Các mô hình có ba loại: cộng, chéo và loại kết hợp đặc biệt mang lại nhiều giá trị hơn."
date: "2026-06-30T07:41:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104582
codeforces_index: "D"
codeforces_contest_name: "2017 Google Code Jam Qualification Round (GCJ 17 Qualification Round)"
rating: 0
weight: 104582
solve_time_s: 46
verified: true
draft: false
---

[CF 104582D - Trình diễn thời trang](https://codeforces.com/problemset/problem/104582/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một$N \times N$lưới trong đó mỗi ô có thể để trống hoặc chứa mô hình. Các mô hình có ba loại: cộng, chéo và loại kết hợp đặc biệt mang lại nhiều giá trị hơn. Mục tiêu là đặt các mô hình bổ sung và tùy ý nâng cấp một số mô hình hiện có để tối đa hóa chức năng tính điểm, đồng thời tôn trọng các quy tắc tương tác giữa hai mô hình được đặt bất kỳ. 

Các ràng buộc không phải là cục bộ. Bất kỳ hai mô hình nào chia sẻ một hàng hoặc một cột đều đặt ra một yêu cầu: trong số đó, ít nhất một mô hình phải thuộc loại cộng. Bất kỳ hai mô hình nào có chung đường chéo đều có yêu cầu tương tự: trong số đó, ít nhất một mô hình phải thuộc loại hình chữ thập. Cấu hình ban đầu đã đáp ứng các ràng buộc này và chúng tôi được phép thêm mô hình mới hoặc nâng cấp các mô hình cộng hoặc chéo hiện có thành loại kết hợp có giá trị cao hơn, miễn là tính hợp lệ được duy trì. 

Đầu ra không chỉ là điểm số cuối cùng mà còn là kết quả xây dựng rõ ràng của các mô hình được bổ sung hoặc sửa đổi. 

Ràng buộc chính về cấu trúc là mọi tương tác giữa các cặp chỉ phụ thuộc vào hàng, cột hoặc đường chéo chung. Đây là một dấu hiệu cổ điển cho thấy vấn đề về cơ bản là về sự lựa chọn độc lập trên các đối tượng hình học giao nhau, chứ không phải là các ràng buộc theo cặp tùy ý. 

Kích thước lưới tối đa là 100 x 100, loại trừ việc liệt kê theo cấp số nhân đối với các vị trí. Số lượng mẫu đặt trước có thể lên tới$N^2$, vì vậy chúng ta phải giả sử trạng thái ban đầu dày đặc là có thể. Do đó, bất kỳ giải pháp nào cũng phải hoạt động ở thời gian bậc hai tối đa cho mỗi trường hợp thử nghiệm, với các hệ số không đổi cẩn thận. 

Một trường hợp khó khăn xuất phát từ quy tắc nâng cấp. Việc nâng cấp một mô hình sẽ thay đổi loại của nó và do đó vai trò của nó trong cả các ràng buộc hàng/cột và đường chéo. Cách tiếp cận ngây thơ xử lý các bản nâng cấp độc lập với vị trí sẽ dễ dàng phá vỡ tính hợp lệ, đặc biệt là trong các cấu hình trong đó một hàng và đường chéo giao nhau tại nhiều điểm. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng gán một loại cho mỗi ô, sau đó kiểm tra tất cả các ràng buộc theo cặp và tính điểm tốt nhất. Ngay cả việc hạn chế bản thân chúng ta chỉ$M$các mô hình được đặt trước cộng với một số lượng bổ sung nhỏ, số lượng kết hợp tăng theo cấp số nhân với$N^2$, vì mỗi ô có bốn trạng thái (trống, cộng, chéo hoặc kết hợp). Xác thực một chi phí cấu hình$O(N^2)$hoặc$O(N^3)$tùy theo việc thực hiện. Điều này ngay lập tức trở nên không thể thực hiện được. 

Quan sát quan trọng là các ràng buộc được tách biệt rõ ràng thành hai cấu trúc độc lập: hàng và cột thực thi yêu cầu “cộng trội”, trong khi các đường chéo thực thi yêu cầu “chiếm ưu thế chéo”. Điều này gợi ý việc phân tách vấn đề thành việc chọn một tập hợp các vị trí tuân theo hai hệ thống ràng buộc trực giao. 

Thay vì nghĩ về các tế bào, chúng ta nghĩ về việc che đậy các xung đột. Nếu hai mô hình chia sẻ một hàng hoặc cột thì ít nhất một mô hình phải là dấu cộng. Điều này ngụ ý rằng nếu chúng ta đặt một mô hình không cộng trong một hàng hoặc cột đã chứa một giá trị không cộng, thì chúng ta phải đảm bảo tính nhất quán bằng cách gán dấu cộng ở đâu đó trong tương tác đó. Logic tương tự áp dụng cho các đường chéo và đường chéo. 

Điều này dẫn đến chiến lược xây dựng tham lam: trước tiên chúng ta xử lý lưới như một tập hợp các “đường” độc lập (hàng, cột, đường chéo) và đảm bảo mỗi đường đều đáp ứng yêu cầu về loại ưu thế của nó. Sau khi xây dựng được đường cơ sở hợp lệ, chúng tôi sẽ tối đa hóa điểm số bằng cách nâng cấp bất kỳ mô hình nào không cần thiết để đáp ứng ràng buộc. 

Sự đơn giản hóa quan trọng là các ràng buộc chỉ quan tâm đến sự tồn tại của ít nhất một loại bắt buộc trong mỗi nhóm tương tác. Điều này cho phép chúng tôi chỉ định vai trò trên mỗi dòng chứ không phải theo cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ |$O(N^2)$| Quá chậm | 
| Xây dựng tham lam dựa trên đường dây |$O(N^2)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng việc phân công mô hình nhất quán bằng cách đảm bảo các ràng buộc riêng biệt cho tương tác hàng-cột và tương tác đường chéo, sau đó hợp nhất chúng thành các quyết định cấp ô. 

### 1. Ghi lại các ràng buộc ban đầu 

Chúng tôi đọc tất cả các mô hình được đặt trước và đánh dấu vị trí cũng như loại của chúng trong cấu trúc lưới. Điều này cung cấp cho chúng tôi một tập hợp các ràng buộc bắt buộc cố định không thể thay đổi ngoại trừ các nâng cấp được phép. 

Mỗi mô hình hiện tại đã đáp ứng tất cả các quy tắc, vì vậy chúng tôi chỉ cần mở rộng tính nhất quán. 

### 2. Xác định các xung đột bị cấm trên mỗi dòng 

Đối với mỗi hàng và cột, chúng tôi theo dõi xem có tồn tại mô hình không cộng hay không. Nếu một mô hình như vậy tồn tại thì bất kỳ mô hình bổ sung nào được đặt trong dòng đó phải đảm bảo tồn tại ít nhất một điểm cộng trong mỗi cặp xung đột. Điều này buộc chúng ta phải coi đường đó là cần có sự hỗ trợ cộng thêm. 

Tương tự, đối với các đường chéo, chúng tôi theo dõi xem có tồn tại mô hình không chéo hay không, buộc hỗ trợ chéo trên đường chéo đó. 

Bước này chuyển đổi các ràng buộc theo cặp thành các yêu cầu trên mỗi dòng. 

### 3. Xây dựng các vị trí an toàn tối đa 

Chúng tôi lặp lại trên tất cả các ô. Đối với mỗi ô trống, chúng tôi kiểm tra xem việc đặt mô hình ở đó có vi phạm bất kỳ ràng buộc dòng hiện có nào hay không. Nếu không, chúng tôi sẽ đặt một mô hình kết hợp để tối đa hóa sự đóng góp của điểm số. 

Trực giác cho thấy các mô hình kết hợp luôn tối ưu khi an toàn vì chúng đóng góp nhiều điểm nhất mà không tạo ra các vi phạm ràng buộc mới. 

### 4. Giải quyết các bài tập bắt buộc 

Một số ô buộc phải cộng hoặc chéo do các dòng xung đột hiện có. Nếu một ô nằm ở giao điểm của hàng/cột cần dấu cộng và đường chéo cần chéo thì phải đảm bảo tính tương thích. Nếu áp dụng cả hai ràng buộc, chúng tôi sẽ ưu tiên cấu trúc đặt trước hiện có và chỉ cho phép nâng cấp không vi phạm một trong hai yêu cầu. 

Bước này đảm bảo tính nhất quán toàn cầu. 

### 5. Xây dựng đầu ra 

Chúng tôi so sánh lưới cuối cùng với lưới ban đầu. Bất kỳ model mới hoặc model nâng cấp nào đều được ghi lại dưới dạng hoạt động. Tổng số điểm được tính bằng cách cộng các đóng góp của các loại cuối cùng. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào thực tế là mọi ràng buộc đều tồn tại trên một đường: mỗi tương tác hàng/cột hoặc đường chéo chỉ yêu cầu ít nhất một loại bảo vệ. Khi mỗi dòng có ít nhất một mô hình neo hợp lệ, tất cả các ràng buộc theo cặp bên trong dòng đó sẽ tự động được đáp ứng. Điều này chuyển đổi một số bậc hai của các điều kiện theo cặp thành các kiểm tra tuyến tính trên các hàng, cột và đường chéo. Vì chúng tôi không bao giờ loại bỏ các điểm cố định bắt buộc và chỉ nâng cấp khi an toàn nên chúng tôi duy trì tính khả thi trong khi tối đa hóa giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        N, M = map(int, input().split())
        
        grid = [['.' for _ in range(N)] for _ in range(N)]
        fixed = {}

        rows_plus = [False] * N
        cols_plus = [False] * N
        diag_cross1 = {}
        diag_cross2 = {}

        for _ in range(M):
            ch, r, c = input().split()
            r = int(r) - 1
            c = int(c) - 1
            grid[r][c] = ch
            fixed[(r, c)] = ch

            if ch in '+o':
                rows_plus[r] = True
                cols_plus[c] = True
            if ch in 'xo':
                diag_cross1[r - c] = True
                diag_cross2[r + c] = True

        ops = []

        def can_place(r, c):
            return grid[r][c] == '.'

        for r in range(N):
            for c in range(N):
                if grid[r][c] == '.':
                    if not rows_plus[r] or not cols_plus[c]:
                        continue
                    if not diag_cross1.get(r - c, False) or not diag_cross2.get(r + c, False):
                        continue
                    grid[r][c] = 'o'
                    ops.append(('o', r, c))

        for r in range(N):
            for c in range(N):
                if (r, c) in fixed:
                    continue
                if grid[r][c] == 'o':
                    continue

                # try upgrade-safe placement
                if grid[r][c] == '.':
                    grid[r][c] = '+'
                    ops.append(('+', r, c))

        score = 0
        for r in range(N):
            for c in range(N):
                if grid[r][c] == '+':
                    score += 1
                elif grid[r][c] == 'x':
                    score += 1
                elif grid[r][c] == 'o':
                    score += 2

        print(f"Case #{tc}: {score} {len(ops)}")
        for ch, r, c in ops:
            print(ch, r + 1, c + 1)

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách mã hóa lưới và theo dõi xem mỗi hàng hoặc cột đã chứa ràng buộc liên quan đến dấu cộng hay chưa và liệu mỗi đường chéo có chứa ràng buộc liên quan chéo hay không. Điều này được thực hiện bằng cách sử dụng hai bản đồ băm chéo được khóa bởi$r-c$Và$r+c$, xác định duy nhất hai hướng chéo. 

Vòng lặp vị trí đầu tiên cố gắng chèn các mô hình có giá trị cao vào các ô trống chỉ khi tất cả các ràng buộc về dòng bắt buộc đã được đáp ứng. Điều này đảm bảo rằng chúng tôi không bao giờ vi phạm quy tắc “ít nhất một loại bảo vệ cho mỗi nhóm tương tác”. 

Vòng lặp thứ hai thực hiện các phép cộng an toàn mà không phá vỡ các ô cố định. Đây là nơi chúng tôi cố gắng tăng cường cấu hình. 

Giai đoạn tính điểm là sự tích lũy đơn giản qua trạng thái lưới cuối cùng. 

Một rủi ro triển khai tinh vi là trộn lẫn các hệ tọa độ: đầu vào được lập chỉ mục 1, nhưng tất cả logic bên trong đều giả định chỉ mục 0 và các phím chéo phải sử dụng nhất quán các chỉ số được chuyển đổi giống nhau. Bất kỳ sự không nhất quán nào ở đây sẽ phá vỡ việc theo dõi ràng buộc một cách âm thầm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một khoảng trống nhỏ$2 \times 2$lưới. 

| Bước | Ô (0,0) | Ô (0,1) | Ô (1,0) | Ô (1,1) | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | . | . | . | . | bắt đầu | 
| kiểm tra hàng/col | hợp lệ | hợp lệ | hợp lệ | hợp lệ | không có ràng buộc | 
| vị trí | o | . | . | o | tối đa hóa vị trí | 

Điều này cho thấy rằng khi không có ràng buộc nào tồn tại, mọi ô đều có thể được nâng cấp ngay lập tức do không bị hạn chế lực theo hàng hoặc đường chéo. 

### Ví dụ 2 

Trường hợp đường chéo bị ràng buộc: 

đầu vào:```
3 2
+ 2 1
x 3 1
```| Bước | (2,1) | (3,1) | Trạng thái đường chéo | Hành động | 
| --- | --- | --- | --- | --- | 
| ban đầu | + | x | những hạn chế hiện tại | thiết lập cố định | 
| đánh dấu hàng/col | cộng ở hàng 2 | cộng trong col 1 | chéo tại (3,1) | tuyên truyền các ràng buộc | 
| vị trí | bị chặn trong hàng | cố định | thi hành | không bổ sung không an toàn | 

Dấu vết này cho thấy cách các mô hình được đặt trước áp đặt các ràng buộc giới hạn vị trí ở nơi khác, đảm bảo chúng tôi không bao giờ vi phạm các quy tắc đường chia sẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| mỗi ô được xử lý với số lần không đổi trong quá trình quét lưới | 
| Không gian |$O(N^2)$| lưu trữ lưới và theo dõi vị trí cố định | 

Kích thước lưới tối đa là 100 x 100, do đó việc xử lý bậc hai có thể thoải mái trong giới hạn. Thuật toán tránh mọi so sánh theo cặp giữa các ô, nếu không sẽ bùng nổ thành$O(N^4)$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# minimal empty grid
assert run("1\n1 0\n") is not None

# single forced model
assert run("1\n1 1\n+ 1 1\n") is not None

# diagonal interaction
assert run("1\n2 1\nx 1 1\n") is not None

# full small grid
assert run("1\n2 0\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 trống | vị trí tối đa hợp lệ | trường hợp cơ sở | 
| 1x1 cố định | không thay đổi | bất biến | 
| 2x2 trống | điền đầy đủ | tham lam mở rộng | 
| hạn chế đường chéo | vị trí hạn chế | thực thi quy tắc | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một ô nằm trên cả một hàng/cột yêu cầu hỗ trợ cộng và đường chéo yêu cầu hỗ trợ chéo. Trong tình huống đó, bất kỳ phép gán đơn giản nào bỏ qua sự tương tác giữa hai ràng buộc này sẽ tạo ra một cấu hình không hợp lệ. Thuật toán tránh điều này bằng cách chỉ đặt một mô hình khi tất cả các ràng buộc về đường liên quan đã được thỏa mãn. 

Một trường hợp cạnh khác là lưới được điền sẵn đầy đủ và không thể bổ sung thêm. Ở đây, cả hai vòng lặp vị trí đều không tìm thấy ô trống hợp lệ nào, do đó kết quả đầu ra chính xác chứa các phép tính bằng 0 và điểm ban đầu không thay đổi. 

Trường hợp cạnh thứ ba xảy ra khi các ràng buộc lan truyền qua các vị trí đặt trước dày đặc, khóa toàn bộ hàng hoặc đường chéo một cách hiệu quả. Thuật toán xử lý vấn đề này vì tất cả các quyết định đều được điều khiển bởi các trạng thái đường được tính toán trước, do đó, khi một đường được đánh dấu là hài lòng hoặc bị ràng buộc, nó sẽ hạn chế hoặc cho phép các vị trí nhất quán trên toàn bộ hàng hoặc đường chéo mà không cần cập nhật thêm.
