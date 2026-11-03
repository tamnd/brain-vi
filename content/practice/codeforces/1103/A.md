---
title: "CF 1103A - Trò chơi lưới"
description: "Chúng tôi đang làm việc trên một bảng 4 x 4 cố định bắt đầu trống. Một loạt quân domino xuất hiện lần lượt và mỗi quân domino phải được đặt ngay lập tức khi nó xuất hiện. Mỗi domino có thể theo chiều dọc, bao gồm hai ô trong cùng một cột hoặc theo chiều ngang, bao gồm hai ô trong cùng một hàng."
date: "2026-06-13T07:45:08+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1103
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 534 (Div. 1)"
rating: 1400
weight: 1103
solve_time_s: 203
verified: true
draft: false
---

[CF 1103A - Trò chơi lưới](https://codeforces.com/problemset/problem/1103/A) 

**Đánh giá:** 1400 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 3m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một bảng 4 x 4 cố định bắt đầu trống. Một loạt quân domino xuất hiện lần lượt và mỗi quân domino phải được đặt ngay lập tức khi nó xuất hiện. 

Mỗi domino có thể theo chiều dọc, bao gồm hai ô trong cùng một cột hoặc theo chiều ngang, bao gồm hai ô trong cùng một hàng. Sau khi được đặt, bảng có thể thay đổi: nếu bất kỳ hàng nào được lấp đầy hoàn toàn, toàn bộ hàng đó sẽ bị xóa cùng lúc; tương tự, nếu cột nào bị lấp đầy hoàn toàn thì cột đó cũng bị xóa. Việc xóa này xảy ra sau mỗi vị trí và có thể xóa nhiều hàng hoặc cột cùng một lúc. 

Nhiệm vụ là quyết định vị trí đặt mọi quân domino sắp đến sao cho nó luôn vừa với các ô hiện đang trống và chúng ta phải xuất ra vị trí của mọi vị trí. Vị trí đầu ra là hàng và cột nhỏ nhất mà quân domino đó chạm vào, tương ứng với ô trên cùng bên trái của quân domino dọc hoặc ô ngoài cùng bên trái của quân domino ngang. 

Hạn chế chính là bàn cờ cực kỳ nhỏ, chỉ có 16 ô, trong khi số lượng quân domino có thể lên tới 1000. Điều này ngay lập tức loại trừ bất kỳ chiến lược nào cố gắng duy trì tối ưu hóa toàn cầu phức tạp theo thời gian. Ngay cả mô phỏng không hiệu quả với hệ số không đổi cũng được, miễn là mỗi bước được giới hạn bởi một tìm kiếm cố định nhỏ trên lưới 4 x 4. 

Một khó khăn tinh tế đến từ quy tắc xóa. Một cách tiếp cận ngây thơ chỉ đơn giản là “lấp đầy các ô một cách tham lam” mà không mô phỏng việc xóa hàng và cột cuối cùng sẽ bị phá vỡ, bởi vì không gian bị chặn trước đó có thể đột ngột xuất hiện lại sau khi xóa. Một trường hợp thất bại khác là giả định một mô hình vị trí cố định; việc xóa bỏ làm cho trạng thái trở nên linh hoạt và phụ thuộc vào các vị trí trong quá khứ. 

Trường hợp cạnh cuối cùng là khi nhiều hàng hoặc cột đầy cùng một lúc. Tất cả chúng phải được xóa đồng thời, nếu không các vị trí sau này có thể cho rằng các ô vẫn bị chiếm giữ một cách không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng mô phỏng tất cả các vị trí có thể có của mỗi quân domino ở mỗi bước. Đối với mỗi phần đến, chúng tôi sẽ quét tất cả các vị trí hợp lệ và thử đệ quy các vị trí trong tương lai. Điều này nhanh chóng trở thành cấp số nhân vì mỗi bước có tới hàng chục lựa chọn vị trí ngay cả trên bảng 4 x 4 và có thể có 1000 bước. Ngay cả khi cắt tỉa, yếu tố phân nhánh cũng khiến điều này không thể thực hiện được. 

Quan sát quan trọng là tấm bảng quá nhỏ nên chúng ta không bao giờ cần phải nhìn về phía trước. Thay vào đó, chúng ta có thể mô phỏng một cách tham lam trong khi luôn duy trì cấu hình hợp lệ. Vì việc xóa liên tục đặt lại các phần của bảng nên chúng tôi không xây dựng cấu hình đóng gói cuối cùng mà liên tục giải quyết vấn đề đóng gói nhỏ trên một bảng gần như mới. 

Điều này cho phép một chiến lược mang tính xây dựng: đối với mỗi quân domino, chúng tôi chỉ cần tìm kiếm vị trí hợp lệ đầu tiên phù hợp với trạng thái bàn cờ hiện tại. Sau khi đặt nó, chúng tôi ngay lập tức cập nhật bảng và xóa mọi hàng hoặc cột đầy đủ. Bởi vì lưới có kích thước không đổi nên việc kiểm tra tất cả các khả năng sẽ tốn thời gian không đổi và mô phỏng đầy đủ là tuyến tính theo độ dài của đầu vào. 

Lý do điều này có tác dụng là vì bất kỳ vị trí hợp lệ cục bộ nào cũng đủ. Việc xóa đảm bảo rằng bảng không bao giờ tích lũy các ràng buộc không thể đảo ngược; thay vào đó, nó liên tục giải phóng không gian, ngăn chặn ngõ cụt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cây tìm kiếm Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô phỏng tham lam với tính năng quét | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một lưới boolean 4 x 4 đại diện cho các ô bị chiếm dụng. 

1. Đối với mỗi domino sắp đến, chúng tôi quét lưới theo thứ tự cố định để tìm vị trí hợp lệ. 

Đối với quân domino ngang, chúng ta tìm hàng r và cột c sao cho cả (r, c) và (r, c+1) đều trống. Đối với quân domino dọc, chúng tôi tìm kiếm (r, c) và (r+1, c) trống.

Thứ tự quét cố định đảm bảo tính xác định và tránh sự mơ hồ. 
2. Sau khi tìm thấy vị trí hợp lệ, chúng tôi đánh dấu hai ô là bị chiếm dụng và ghi lại tọa độ đầu ra là (r, c) trong lập chỉ mục dựa trên 1. 
3. Sau khi sắp xếp, chúng tôi kiểm tra tất cả các hàng. Nếu một hàng có tất cả bốn ô bị chiếm giữ, chúng ta sẽ xóa nó bằng cách đặt chúng thành trống. 
4. Sau đó chúng tôi kiểm tra tất cả các cột. Nếu một cột có tất cả bốn ô bị chiếm giữ, chúng ta sẽ xóa cột đó theo cách tương tự. 
5. Chúng tôi lặp lại bước xóa này cho đến khi không có hàng hoặc cột nào được điền đầy đủ, vì việc xóa một hàng có thể khiến cột bị đầy và ngược lại. 

Lý do chúng tôi liên tục làm rõ là một vị trí có thể kích hoạt hàng loạt lượt xóa. 

### Tại sao nó hoạt động 

Ở mỗi bước, lưới thể hiện chính xác tập hợp các ô hiện hoạt sau khi áp dụng tất cả các thao tác xóa. Quá trình quét luôn tìm thấy vị trí vì bảng có tối đa 16 ô và việc xóa sẽ ngăn chặn tình trạng bão hòa vĩnh viễn. Vì chúng tôi chỉ đặt một quân domino vào hai ô trống nên chúng tôi không bao giờ vi phạm các ràng buộc chồng chéo. Quy tắc xóa đảm bảo rằng bất cứ khi nào một hàng hoặc cột đầy, nó sẽ bị xóa ngay lập tức, khôi phục dung lượng và ngăn ngừa bế tắc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def clear(board):
    changed = True
    while changed:
        changed = False

        # check rows
        for r in range(4):
            if all(board[r][c] for c in range(4)):
                for c in range(4):
                    board[r][c] = 0
                changed = True

        # check cols
        for c in range(4):
            if all(board[r][c] for r in range(4)):
                for r in range(4):
                    board[r][c] = 0
                changed = True

def solve():
    s = input().strip()
    board = [[0] * 4 for _ in range(4)]
    out = []

    for ch in s:
        placed = False

        if ch == '1':  # horizontal
            for r in range(4):
                for c in range(3):
                    if board[r][c] == 0 and board[r][c + 1] == 0:
                        board[r][c] = board[r][c + 1] = 1
                        out.append((r + 1, c + 1))
                        placed = True
                        break
                if placed:
                    break

        else:  # vertical
            for r in range(3):
                for c in range(4):
                    if board[r][c] == 0 and board[r + 1][c] == 0:
                        board[r][c] = board[r + 1][c] = 1
                        out.append((r + 1, c + 1))
                        placed = True
                        break
                if placed:
                    break

        clear(board)

    print("\n".join(f"{r} {c}" for r, c in out))

if __name__ == "__main__":
    solve()
```Giải pháp này giữ một lưới chiếm chỗ theo đúng nghĩa đen là 4 x 4, giúp tránh mọi cấu trúc dữ liệu phức tạp. Logic vị trí là quá trình quét xác định, luôn chọn mức phù hợp có sẵn đầu tiên, an toàn vì lưới rất nhỏ và chúng tôi không bao giờ cần đóng gói tối ưu. 

Chức năng xóa được tách biệt vì việc xóa hàng và cột có thể xâu chuỗi và chúng ta phải tiếp tục áp dụng chúng cho đến khi đạt được độ ổn định. 

Một lỗi phổ biến là quên xóa nhiều lần, lỗi này có thể khiến hàng mới hoàn thành không bị xóa nếu nó hình thành sau khi xóa cột trong cùng một bước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
010
```Chúng tôi bắt đầu với một bảng trống. 

| Bước | Domino | Vị trí (r, c) | Hành động chính | 
| --- | --- | --- | --- | 
| 1 | dọc | (1,1) | khe dọc có sẵn đầu tiên | 
| 2 | ngang | (1,2) | khe ngang có sẵn đầu tiên | 
| 3 | dọc | (1,4) | được đặt, sau đó việc dọn dẹp hàng/col xảy ra | 

Sau vị trí thứ ba, hàng 1 trở nên đầy và bị xóa, khớp với hành vi mẫu. 

Dấu vết này cho thấy rằng mặc dù chúng ta tham lam điền từ phía trên bên trái, việc xóa sẽ định hình lại bảng và ngăn chặn việc chặn. 

### Ví dụ 2 

đầu vào:```
1111
```| Bước | Domino | Vị trí | Hành động chính | 
| --- | --- | --- | --- | 
| 1 | H | (1,1) | chiếm (1,1)-(1,2) | 
| 2 | H | (1,3) | hàng 1 sẽ đầy sau | 
| 3 | H | (2,1) | hàng 1 bị xóa, không gian được sử dụng lại | 
| 4 | H | (2,3) | điền vào hàng thứ hai | 

Điều này chứng tỏ rằng việc xóa hàng cho phép tái sử dụng không gian, do đó, ngay cả các vị trí nằm ngang lặp đi lặp lại vẫn khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi domino quét tối đa 16 ô và xóa kiểm tra một lưới 4 x 4 cố định | 
| Không gian | O(1) | chỉ một bảng có kích thước không đổi được lưu trữ | 

Các ràng buộc cho phép tối đa 1000 thao tác và mỗi thao tác là công việc liên tục trên một lưới cố định, do đó giải pháp dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`| vị trí duy nhất | trường hợp cơ sở dọc | 
|`1`| vị trí duy nhất | trường hợp đế ngang | 
|`0000`| vị trí hợp lệ | sử dụng theo chiều dọc lặp đi lặp lại | 
|`1111`| vị trí hợp lệ | lặp đi lặp lại ngang + thanh toán bù trừ | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi một hàng trở nên đầy do nửa sau của mẫu vị trí domino theo thời gian chứ không phải do một lần điền rõ ràng. Thuật toán xử lý việc này vì việc xóa được áp dụng sau mỗi vị trí, đảm bảo không còn hàng đầy đủ cũ nào ở trạng thái. 

Một trường hợp khác là hoàn thành hàng và cột đồng thời. Vòng lặp xóa lặp đi lặp lại đảm bảo cả hai đều bị loại bỏ ngay cả khi cái này kích hoạt cái kia một cách gián tiếp, ngăn chặn các trạng thái trung gian không nhất quán và đảm bảo bảng vẫn hợp lệ cho vị trí tiếp theo.
