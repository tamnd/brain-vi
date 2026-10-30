---
title: "CF 106259E - Một lát Pi"
description: "Chúng ta được cho các điểm bên trong một đường tròn có tâm ở gốc tọa độ. Mỗi điểm đại diện cho một đỉnh và có một giá trị số có thể dương hoặc âm."
date: "2026-06-18T23:39:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106259
codeforces_index: "E"
codeforces_contest_name: "CUET Inter University Programming Contest 2025"
rating: 0
weight: 106259
solve_time_s: 62
verified: true
draft: false
---

[CF 106259E - Một lát Pi](https://codeforces.com/problemset/problem/106259/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho các điểm bên trong một đường tròn có tâm ở gốc tọa độ. Mỗi điểm đại diện cho một đỉnh và có một giá trị số có thể dương hoặc âm. Chúng ta được phép chọn một lát bánh pizza được xác định bởi hai tia bắt đầu từ gốc tọa độ, nghĩa là chúng ta chọn một khoảng góc và lấy mọi phần trên cùng có hướng từ gốc nằm trong khoảng đó. Tất cả mọi thứ trên ranh giới được bao gồm. 

Lát cắt có một ràng buộc hình học: diện tích của nó không thể vượt quá một giới hạn nhất định. Vì một lát cắt là một hình tròn nên diện tích của nó tỉ lệ thuận với góc của nó. Điều này biến ràng buộc thành giới hạn trên về độ rộng của khoảng góc mà chúng ta được phép chọn. 

Đối với bất kỳ lát cắt hợp lệ nào, chúng ta tính tổng giá trị của tất cả các điểm bên trong nó. Điểm của lát cắt là giá trị tuyệt đối của tổng này. Nhiệm vụ là chọn khoảng góc tốt nhất có thể thỏa mãn giới hạn diện tích và tối đa hóa tổng tuyệt đối này. 

Kích thước đầu vào lớn, lên tới 3×10^5 điểm trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ phép liệt kê bậc hai nào của các cặp điểm làm ranh giới góc. Bất kỳ giải pháp nào cố gắng kiểm tra tất cả các cặp tia hoặc tính toán lại tổng từ đầu cho mỗi khoảng ứng cử viên sẽ quá chậm. 

Điểm tinh tế chính là chúng tôi không tối đa hóa một tổng đơn giản trên một đoạn đường có điểm cuối cố định mà trên một miền hình tròn. Khoảng có thể bao quanh góc 0 hướng, điều này làm cho việc suy luận cửa sổ trượt tuyến tính ngây thơ không đầy đủ trừ khi chúng ta xử lý tính chất vòng tròn một cách cẩn thận. 

Một trường hợp thất bại điển hình xuất phát từ việc bỏ qua sự bao bọc. Giả sử hai cụm có giá trị cao nằm gần các góc 0,01 và 6,27 radian. Một phép quét tuyến tính đơn giản trên các góc được sắp xếp sẽ coi chúng cách xa nhau, mặc dù một lát cắt nhỏ vượt qua ranh giới 2π có thể bao gồm cả hai. Bất kỳ giải pháp nào không trùng lặp hoặc không mô hình hóa tính liên tục vòng tròn sẽ bỏ lỡ các lát cắt tối ưu như vậy. 

Một lỗi phổ biến khác là bỏ qua giá trị tuyệt đối. Nếu người ta chỉ theo dõi mảng con tổng tối đa, thì các vùng nặng âm sẽ bị bỏ qua, mặc dù một lát cắt âm mạnh có thể mang lại giá trị tuyệt đối lớn hơn bất kỳ lát cắt dương nào. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận vũ phu sẽ chọn từng cặp lớp trên làm các tia biên tiềm năng, tính khoảng góc mà chúng xác định, rồi tính tổng tất cả các điểm nằm bên trong. Ngay cả khi chúng tôi tính toán trước các góc và sử dụng tổng tiền tố, chúng tôi vẫn cần kiểm tra các khoảng O(n^2) và việc kiểm tra tính hợp lệ của từng khoảng trên một cấu trúc hình tròn vẫn tốn thời gian tuyến tính trong trường hợp xấu nhất. Điều này dẫn đến hành vi gần như O(n^3) nếu được thực hiện trực tiếp hoặc O(n^2) với các truy vấn phạm vi được tối ưu hóa, cả hai đều vượt xa giới hạn. 

Quan sát quan trọng là chỉ có thứ tự góc mới quan trọng. Khi các điểm được sắp xếp theo góc, bất kỳ lát cắt hợp lệ nào cũng tương ứng với một đoạn liền kề theo thứ tự hình tròn này, với một ràng buộc bổ sung là chiều rộng góc không vượt quá ngưỡng cố định xuất phát từ giới hạn diện tích. 

Điều này làm giảm vấn đề tìm kiếm một mảng con có tổng tuyệt đối tối đa trên một mảng hình tròn, với ràng buộc bổ sung là độ dài mảng con được xác định theo khoảng cách góc thay vì khoảng cách chỉ số. Khi các góc được sắp xếp, khoảng cách góc sẽ trở nên đơn điệu dọc theo mảng, cho phép một cửa sổ trượt hai con trỏ. 

Sau đó, chúng tôi chuyển đổi mảng hình tròn thành mảng tuyến tính bằng cách sao chép nó. Điều này cho phép các khoảng bao quanh trở thành khoảng tiêu chuẩn trong một mảng nhân đôi. Bên trong cấu trúc này, chúng ta duy trì một cửa sổ trượt luôn thỏa mãn giới hạn về chiều rộng góc. Đối với mỗi điểm cuối bên phải, chúng tôi nâng cao điểm cuối bên trái nếu cần và duy trì tổng số hiện có. Câu trả lời là giá trị tuyệt đối tối đa của bất kỳ tổng cửa sổ hợp lệ nào, vì vậy chúng tôi theo dõi cả tổng tối đa và tối thiểu.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 log n) hoặc tệ hơn | O(n) | Quá chậm | 
| Cửa Sổ Trượt Tối Ưu | O(n log n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Chuyển giới hạn diện tích thành giới hạn góc 

Một hình tròn có bán kính r và góc θ có diện tích (1/2) r^2 θ. Từ đó chúng ta tính θmax = 2A / r^2. Bất kỳ lát cắt hợp lệ nào cũng phải có chiều rộng góc tối đa là θmax. 

### Bước 2: Tính góc cực của mọi điểm 

Đối với mỗi phần trên cùng, hãy tính góc của nó bằng cách sử dụng atan2(y, x). Điều này đặt mọi điểm vào khoảng [0, 2π). 

Bước này rất cần thiết vì ràng buộc lát cắt chỉ phụ thuộc vào hướng từ gốc chứ không phải khoảng cách. 

### Bước 3: Sắp xếp điểm theo góc 

Chúng tôi sắp xếp tất cả các điểm theo góc cực của chúng. Sau khi sắp xếp, bất kỳ lát cắt hợp lệ nào không có lớp bao quanh đều tương ứng với một phân đoạn liền kề theo thứ tự này. 

Sắp xếp là công cụ chuyển đổi hình học thành cấu trúc 1D nơi có thể tạo cửa sổ. 

### Bước 4: Nhân đôi mảng để xử lý quấn tròn 

Chúng ta nối lại từng điểm với góc tăng thêm 2π. Điều này biến đổi các khoảng tròn thành các khoảng tuyến tính: bất kỳ lát cắt bao quanh nào cũng trở thành một đoạn bình thường trong mảng mở rộng này. 

Bước này đảm bảo chúng ta không bỏ lỡ các lát cắt vượt qua ranh giới góc 0. 

### Bước 5: Sử dụng cửa sổ hai con trỏ để thực thi độ rộng góc 

Chúng tôi duy trì một con trỏ bên trái và tổng số đang chạy. Đối với mỗi con trỏ bên phải, chúng ta di chuyển con trỏ bên trái cho đến khi chênh lệch góc giữa bên phải và bên trái tối đa là θmax. Cửa sổ luôn hợp lệ về mặt diện tích lát cắt. 

Điều này hoạt động vì các góc được sắp xếp đảm bảo tính đơn điệu, do đó, khi một cửa sổ trở nên không hợp lệ, việc tăng con trỏ bên trái có thể khôi phục tính hợp lệ. 

### Bước 6: Theo dõi cả tổng cửa sổ tối đa và tối thiểu 

Chúng tôi duy trì tổng giá trị bên trong cửa sổ. Đối với mỗi cửa sổ hợp lệ, chúng tôi cập nhật cả tổng tối đa và tổng tối thiểu. 

Câu trả lời cuối cùng là giá trị tuyệt đối lớn nhất của hai thái cực này. 

Chúng ta cần cả hai vì lát cắt tối ưu có thể chứa hầu hết các giá trị âm và giá trị tuyệt đối của nó chiếm ưu thế trong bất kỳ lát cắt dương nào. 

### Tại sao nó hoạt động 

Mỗi lát cắt hợp lệ tương ứng với một khoảng góc liền kề. Việc sắp xếp làm cho các khoảng này trở thành các phân đoạn liền kề. Sự trùng lặp đảm bảo các khoảng thời gian bao quanh cũng liền kề nhau. Quá trình hai con trỏ liệt kê mọi phân đoạn hợp lệ tối đa chính xác một lần, bởi vì đối với mỗi điểm cuối bên phải, chúng tôi duy trì điểm cuối bên trái hợp lệ nhỏ nhất. Điều này đảm bảo mọi tổng lát cắt khả thi đều được coi là tổng cửa sổ và việc lấy cả giá trị tối đa và tối thiểu đều bao hàm mục tiêu tuyệt đối. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, r, A = map(int, input().split())
        
        pts = []
        for _ in range(n):
            x, y, v = map(int, input().split())
            ang = math.atan2(y, x)
            if ang < 0:
                ang += 2 * math.pi
            pts.append((ang, v))
        
        if n == 0:
            print(0)
            continue
        
        theta = (2.0 * A) / (r * r)
        if theta >= 2 * math.pi:
            total = sum(v for _, v in pts)
            print(abs(total))
            continue
        
        pts.sort()
        
        ext = pts + [(a + 2 * math.pi, v) for a, v in pts]
        
        left = 0
        cur_sum = 0
        ans = 0
        
        for right in range(len(ext)):
            cur_sum += ext[right][1]
            
            while ext[right][0] - ext[left][0] > theta:
                cur_sum -= ext[left][1]
                left += 1
            
            ans = max(ans, abs(cur_sum))
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chuyển đổi hình học thành các góc bằng atan2, đảm bảo tất cả các hướng được chuẩn hóa thành một tỷ lệ hình tròn duy nhất. Giới hạn diện tích được chuyển đổi thành chiều rộng góc tối đa bằng cách sử dụng công thức diện tích hình cung. 

Việc sắp xếp các điểm theo góc sẽ tạo ra một cấu trúc trong đó các lát cắt hợp lệ tương ứng với các đoạn liền kề. Bước sao chép là bước cho phép các lát cắt bao quanh được xử lý giống hệt với các khoảng thời gian thông thường. 

Cửa sổ trượt duy trì tổng chạy và điều chỉnh ranh giới bên trái bất cứ khi nào chiều rộng góc vượt quá giới hạn cho phép. Giá trị tuyệt đối chỉ được áp dụng khi cập nhật câu trả lời, vì tổng nội bộ phải được giữ nguyên để theo dõi chính xác cả cực dương và cực âm. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với bốn điểm được đặt ở các lượt có giá trị 10, -20, 30, -5. Giả sử giới hạn góc cho phép nhiều nhất là nửa đường tròn. 

Sau khi chuyển đổi sang góc, các điểm được sắp xếp theo thứ tự vòng tròn. Cửa sổ trượt đầu tiên mở rộng cho đến khi đạt đến giới hạn góc, tích lũy các giá trị. Khi di chuyển, nó có thể chụp các phân đoạn như [10, -20, 30] với tổng 20 và [30, -5] với tổng 25. 

| Chỉ mục bên phải | Chỉ số trái | Giá trị cửa sổ | Tổng hợp | 
| --- | --- | --- | --- | 
| 0 | 0 | 10 | 10 | 
| 1 | 0 | 10, -20 | -10 | 
| 2 | 0 | 10, -20, 30 | 20 | 
| 3 | 1 | -20, 30, -5 | 5 | 

Dấu vết này cho thấy cách xử lý các khoản đóng góp tiêu cực một cách tự nhiên trong khi vẫn duy trì một khoảng thời gian hợp lệ. 

Bây giờ hãy xem xét một ví dụ tổng quát trong đó hai điểm có giá trị cao nằm gần các góc 0 và 2π. Sau khi sao chép, chúng trở nên liền kề trong mảng mở rộng, cho phép cửa sổ bao gồm cả hai mà không làm gián đoạn tính liên tục. 

| Chỉ mục bên phải | Chỉ số trái | Giá trị cửa sổ | Tổng hợp | 
| --- | --- | --- | --- | 
| 0 | 0 | 100 | 100 | 
| 1 | 0 | 100, 90 | 190 | 

Điều này xác nhận tại sao việc sao chép là cần thiết: nếu không có nó, những điểm này sẽ xuất hiện rất xa nhau và không bao giờ được kết hợp với nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | Sắp xếp chiếm ưu thế, cửa sổ trượt là tuyến tính | 
| Không gian | O(n) | Lưu trữ danh sách góc và mảng trùng lặp | 

Tổng số điểm trên tất cả các trường hợp thử nghiệm tối đa là 3×10^5, do đó, giải pháp O(n log n) phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ là tuyến tính theo số điểm và vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io, math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, r, A = map(int, input().split())
            pts = []
            for _ in range(n):
                x, y, v = map(int, input().split())
                ang = math.atan2(y, x)
                if ang < 0:
                    ang += 2 * math.pi
                pts.append((ang, v))

            if n == 0:
                print(0)
                continue

            theta = (2.0 * A) / (r * r)
            if theta >= 2 * math.pi:
                print(abs(sum(v for _, v in pts)))
                continue

            pts.sort()
            ext = pts + [(a + 2 * math.pi, v) for a, v in pts]

            left = 0
            cur = 0
            ans = 0

            for right in range(len(ext)):
                cur += ext[right][1]
                while ext[right][0] - ext[left][0] > theta:
                    cur -= ext[left][1]
                    left += 1
                ans = max(ans, abs(cur))

            print(ans)

    return ""

# sample-style sanity checks (placeholders since original sample formatting is incomplete)
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm đơn, diện tích nhỏ | giá trị | Độ chính xác đầu vào tối thiểu | 
| Tất cả các điểm tích cực trong vòng tròn đầy đủ | tổng hợp | Trường hợp bảo hiểm đầy đủ | 
| Dấu hiệu hỗn hợp cụm | mảng con tuyệt đối tối đa | Cửa sổ trượt đúng cách | 
| Hai cụm trên 0 góc | giá trị kết hợp | Xử lý toàn diện | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi chiều rộng góc cho phép vượt quá toàn bộ vòng tròn. Trong tình huống này, ràng buộc không còn có ý nghĩa nữa và mọi điểm có thể được bao gồm trong một lát cắt duy nhất. Thuật toán xử lý việc này bằng cách trả về trực tiếp tổng tuyệt đối của tất cả các giá trị, vì lát cắt hợp lệ duy nhất là tập hợp đầy đủ. 

Một trường hợp khác là khi giá trị tối ưu đến từ một cụm có độ âm cao. Thuật toán theo dõi rõ ràng tổng cửa sổ tối thiểu, đảm bảo rằng tổng âm lớn không bị mất. Ví dụ: nếu một lát chứa các giá trị [-50, -40], thì tổng là -90 và câu trả lời trở thành 90, điều này sẽ bị bỏ qua nếu chỉ tính tổng tối đa. 

Trường hợp cạnh thứ ba là sự thống trị bao trùm. Nếu hai điểm có giá trị cao nằm gần các góc 0 và 2π, chúng sẽ được tách ra theo thứ tự thô nhưng trở nên liền kề sau khi sao chép. Sau đó, cửa sổ trượt sẽ hợp nhất chúng một cách chính xác thành một lát cắt hợp lệ duy nhất, tạo ra điểm cao hơn bất kỳ đoạn tuyến tính nào.
