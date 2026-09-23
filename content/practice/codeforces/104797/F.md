---
title: "CF 104797F - Chữ cái"
description: "Chúng ta có một lưới hình chữ nhật chứa các chữ cái viết thường và các ô trống. Theo thời gian, trọng lực tác động lên lưới này nhưng không theo một hướng cố định."
date: "2026-06-28T13:44:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104797
codeforces_index: "F"
codeforces_contest_name: "2021-2022 ICPC Central Europe Regional Contest (CERC 21)"
rating: 0
weight: 104797
solve_time_s: 30
verified: true
draft: false
---

[CF 104797F - Thư](https://codeforces.com/problemset/problem/104797/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật chứa các chữ cái viết thường và các ô trống. Theo thời gian, trọng lực tác động lên lưới này nhưng không theo một hướng cố định. Thay vào đó, lưới được “giải quyết” nhiều lần theo một chuỗi hướng trọng lực, trong đó mỗi pha di chuyển hoàn toàn tất cả các chữ cái đi xa nhất có thể theo hướng hiện tại cho đến khi chúng bị chặn bởi ranh giới hoặc một chữ cái khác. 

Một pha hoạt động giống như một mô phỏng vật lý: mỗi chữ cái trượt độc lập theo một hướng nhất định, nhưng các chữ cái vẫn giữ nguyên thứ tự tương đối dọc theo hướng đó vì chúng không thể đi qua nhau. Sau khi tất cả các chữ cái dừng lại, giai đoạn tiếp theo bắt đầu với hướng trọng lực có thể khác. 

Nhiệm vụ cuối cùng là tính toán cấu hình sau khi áp dụng tất cả K pha trọng lực. 

Các ràng buộc nhỏ: N và M nhiều nhất là 100 và K nhiều nhất là 100. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào có độ phức tạp xung quanh O(KNM) hoặc thậm chí O(KN M log N) đều có khả năng ổn. Tuy nhiên, các giải pháp mô phỏng chuyển động từng ô một trong mỗi bước vẫn an toàn vì lưới rất nhỏ. 

Điểm tinh tế chính là chuyển động không độc lập trên mỗi tế bào theo nghĩa ngây thơ. Nếu chúng ta di chuyển từng chữ cái một cách không chính xác thì những bước đi trước đó có thể ảnh hưởng đến những chữ cái sau đó trong cùng một pha. Ví dụ: trong giai đoạn đi xuống:```
a
b
.
```Nếu chúng ta di chuyển`a`đầu tiên, nó có thể đi qua không chính xác`b`tùy theo thứ tự thực hiện. Giải thích đúng là tất cả các chữ cái trong một cột hoạt động giống như một ngăn xếp được sắp xếp lại toàn bộ. 

Một trường hợp cạnh tinh tế khác là lưới trống hoặc lưới không có chữ cái. Đầu ra phải không thay đổi và mọi logic nén đều phải xử lý chính xác các chuỗi trống. 

Cuối cùng, K = 0 có nghĩa là không có chuyển động nào xảy ra nên lưới ban đầu phải được in chính xác như đã cho. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu trực tiếp sẽ coi mỗi giai đoạn là một bước vật lý và liên tục di chuyển từng chữ cái một ô tại một thời điểm theo hướng trọng lực cho đến khi không thể thực hiện được chuyển động nào. Đối với mỗi giai đoạn, chúng tôi có thể thử thực hiện một số thao tác như quét lưới, di chuyển các chữ cái và lặp lại cho đến khi ổn định. Trong trường hợp xấu nhất, một chữ cái có thể di chuyển các bước O(max(N, M)) mỗi giai đoạn và chúng ta có thể cần nhiều bước để giải quyết các tương tác, dẫn đến độ phức tạp có thể giảm xuống O(KN²M²) khi triển khai không hiệu quả. Điều này là không cần thiết vì các vị trí cuối cùng bên trong mỗi hàng hoặc cột được xác định hoàn toàn bằng cách sắp xếp hoặc nén. 

Quan sát quan trọng là lực hấp dẫn không tạo ra những tương tác phức tạp ngoài trật tự dọc theo một đường thẳng. Theo bất kỳ hướng cố định nào, lưới phân hủy thành các đường độc lập: các cột cho trọng lực thẳng đứng và các hàng cho trọng lực ngang. Trong mỗi dòng, các chữ cái chỉ đơn giản là “đóng gói” về một phía trong khi vẫn giữ nguyên thứ tự tương đối của chúng. Điều này tương đương với việc trích xuất tất cả các chữ cái từ một dòng, sau đó viết chúng lại theo thứ tự từ phía đích, điền vào các ô còn lại bằng dấu chấm. 

Do đó, mỗi giai đoạn có thể được xử lý trong O(NM): chúng tôi xây dựng lại từng hàng hoặc cột một cách độc lập bằng cách thu thập các chữ cái và viết lại. 

Vì K nhiều nhất là 100 nên việc lặp lại quy trình này K lần là đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng bước vũ phu | O(K · N² · M²) trường hợp xấu nhất | O(1) thêm | Quá chậm | 
| Nén dòng mỗi pha | O(K · N · M) | O(NM) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi liên tục mô phỏng từng pha trọng lực, nhưng thay vì di chuyển các chữ cái tăng dần, chúng tôi xây dựng lại lưới theo cách có cấu trúc tùy theo hướng. 

### Quy trình từng bước 

1. Đọc lưới thành cấu trúc có thể thay đổi, chẳng hạn như danh sách các danh sách. 
2. Đối với mỗi hướng trọng lực trong chuỗi, xử lý toàn bộ lưới trong một lần. 
3. Nếu hướng thẳng đứng (xuống): 

Chúng tôi xử lý từng cột một cách độc lập. Đối với mỗi cột, chúng tôi quét từ trên xuống dưới, thu thập tất cả các chữ cái theo thứ tự, sau đó viết lại từ dưới lên trên. Điều này mô phỏng lực hấp dẫn kéo các chữ cái xuống dưới trong khi vẫn giữ nguyên trật tự. 
4. Nếu hướng lên: 

Một lần nữa xử lý từng cột. Chúng tôi thu thập các chữ cái từ trên xuống dưới, sau đó viết chúng lại từ trên xuống dưới bắt đầu từ hàng 0. Điều này sẽ xếp các chữ cái lên trên. 
5. Nếu hướng đi đúng: 

Chúng tôi xử lý từng hàng một cách độc lập. Chúng tôi thu thập các chữ cái từ trái sang phải, sau đó viết chúng ngược lại từ phải sang trái bắt đầu từ cột cuối cùng. 
6. Nếu hướng trái: 

Chúng tôi xử lý từng hàng một cách độc lập. Chúng tôi thu thập các chữ cái từ trái sang phải, sau đó viết lại chúng bắt đầu từ vị trí ngoài cùng bên trái. 
7. Sau khi xử lý tất cả các hàng hoặc cột cho một giai đoạn, lưới sẽ được cập nhật và trở thành đầu vào cho giai đoạn tiếp theo. 

### Tại sao nó hoạt động 

Mỗi pha bảo toàn thứ tự tương đối của các chữ cái dọc theo trục trực giao với chuyển động vì các chữ cái không bao giờ giao nhau. Tác dụng duy nhất của trọng lực là nén tất cả các chữ cái dọc theo một đường về một ranh giới. Do đó, mỗi hàng hoặc cột hoạt động giống như một thùng chứa có thứ tự ổn định trong đó các phần tử được định vị lại nhưng không bao giờ được hoán vị bên trong. Bất biến này đảm bảo rằng việc xây dựng lại từng dòng bằng cách trích xuất và chèn lại các chữ cái khớp chính xác với quy trình vật lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def apply_down(grid, n, m):
    for col in range(m):
        stack = []
        for row in range(n):
            if grid[row][col] != '.':
                stack.append(grid[row][col])
        for row in range(n - 1, -1, -1):
            grid[row][col] = stack.pop() if stack else '.'

def apply_up(grid, n, m):
    for col in range(m):
        stack = []
        for row in range(n):
            if grid[row][col] != '.':
                stack.append(grid[row][col])
        for row in range(n):
            grid[row][col] = stack.pop(0) if stack else '.'

def apply_right(grid, n, m):
    for row in range(n):
        stack = []
        for col in range(m):
            if grid[row][col] != '.':
                stack.append(grid[row][col])
        for col in range(m - 1, -1, -1):
            grid[row][col] = stack.pop() if stack else '.'

def apply_left(grid, n, m):
    for row in range(n):
        stack = []
        for col in range(m):
            if grid[row][col] != '.':
                stack.append(grid[row][col])
        for col in range(m):
            grid[row][col] = stack.pop(0) if stack else '.'

def main():
    n, m, k = map(int, input().split())
    dirs = input().strip()
    grid = [list(input().strip()) for _ in range(n)]

    for d in dirs:
        if d == 'D':
            apply_down(grid, n, m)
        elif d == 'U':
            apply_up(grid, n, m)
        elif d == 'R':
            apply_right(grid, n, m)
        else:
            apply_left(grid, n, m)

    for row in grid:
        print(''.join(row))

if __name__ == "__main__":
    main()
```Giải pháp duy trì lưới dưới dạng danh sách 2D có thể thay đổi. Mỗi giai đoạn xây dựng lại hàng hoặc cột tùy theo hướng. 

Đối với trọng lực hướng xuống và hướng lên, mỗi cột được trích xuất thành một danh sách tuyến tính các chữ cái. Đối với lực hấp dẫn đi xuống, chúng tôi điền từ dưới lên để các chữ cái tích lũy ở các ô thấp nhất hiện có. Để có trọng lực hướng lên, chúng tôi đổ đầy từ trên xuống. 

Đối với trọng lực ngang, các hàng được xử lý tương tự, với trọng lực bên phải lấp đầy từ cạnh phải và trọng lực bên trái lấp đầy từ cạnh trái. 

Một chi tiết tinh tế là chúng ta phải bảo toàn thứ tự trong quá trình trích xuất. Đây là lý do tại sao chúng tôi quét consi
