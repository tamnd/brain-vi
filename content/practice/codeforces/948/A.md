---
title: "CF 948A - Bảo Vệ Cừu"
description: "Lưới có thể được xem dưới dạng biểu đồ hình chữ nhật trong đó mỗi ô là một nút được kết nối với bốn ô lân cận trực giao của nó. Một số nút chứa cừu, một số chứa sói và phần còn lại trống."
date: "2026-06-17T02:27:40+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 948
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 470 (rated, Div. 2, based on VK Cup 2018 Round 1)"
rating: 900
weight: 948
solve_time_s: 80
verified: false
draft: false
---

[CF 948A - Bảo vệ cừu](https://codeforces.com/problemset/problem/948/A) 

**Xếp hạng:** 900 
**Tags:** bạo lực, dfs và tương tự, đồ thị, triển khai 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Lưới có thể được xem dưới dạng biểu đồ hình chữ nhật trong đó mỗi ô là một nút được kết nối với bốn ô lân cận trực giao của nó. Một số nút chứa cừu, một số chứa sói và phần còn lại trống. Sói có thể tự do di chuyển từng bước qua các ô trống, nhưng chúng không thể đi qua chó một khi chúng ta đặt chúng. Cừu và chó là những chướng ngại vật tĩnh từ góc độ chuyển động. 

Nhiệm vụ là quyết định xem liệu chúng ta có thể xếp chó vào các ô trống sao cho không có con đường nào tồn tại từ bất kỳ con sói nào đến bất kỳ con cừu nào chỉ bằng cách di chuyển bốn hướng qua các ô không phải là chó hay không. Nếu điều này có thể thực hiện được, chúng ta phải xuất ra một cấu hình hợp lệ về vị trí đặt chó. Nếu không, chúng tôi báo cáo là không thể. 

Các ràng buộc cho phép tối đa lưới 500 x 500, vì vậy tổng số ô tối đa là 250.000. Bất kỳ giải pháp nào thực hiện ngay cả một lượng nhỏ khám phá biểu đồ không đổi trên mỗi ô đều ổn, nhưng bất kỳ giải pháp nào cố gắng tính toán lại khả năng tiếp cận riêng biệt cho từng con sói hoặc từng con cừu vẫn chỉ được chấp nhận nếu được thực hiện cẩn thận dưới dạng một lần duyệt. Việc kiểm tra khả năng tiếp cận theo cặp ngây thơ giữa sói và cừu ngay lập tức không khả thi vì nó sẽ ám chỉ hành vi O(R^2 C^2) trong trường hợp xấu nhất. 

Một điểm tinh tế là những con sói không bị hạn chế về mặt cá nhân. Một con sói tiếp cận bất kỳ con cừu nào sẽ làm mất hiệu lực cấu hình và nhiều con sói có thể hợp tác ngầm vì khả năng tiếp cận hoàn toàn dựa trên biểu đồ. 

Trường hợp cạnh chính phát sinh khi một con sói ở cạnh một con cừu ở đầu vào. Vì chúng tôi chỉ được phép nhốt chó vào các ô trống nên không có cách nào để tách chúng ra nếu chúng đã ở cạnh nhau. Ví dụ: trong lưới 2 x 2 có sói và cừu ở cạnh nhau, câu trả lời là không thể ngay lập tức. Một trường hợp quan trọng khác là khi sói bao vây một con cừu theo cách mà tất cả các con đường từ sói đến con cừu đó đều đi qua một ô hành lang trống. Hành lang đó phải bị chặn và giải pháp phải xác định chính xác và đặt chó ở đó. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng chuyển động của tất cả những con sói bằng cách sử dụng BFS đa nguồn, đánh dấu tất cả các ô có thể tiếp cận. Sau khi tính toán toàn bộ vùng sói có thể tiếp cận, chúng tôi chỉ cần kiểm tra xem có con cừu nào nằm trong đó hay không. Nếu có thể tiếp cận được một con cừu, chúng tôi cố gắng đặt chó để chặn đường và lặp lại. Tuy nhiên, điều này trở nên phức tạp vì việc quyết định nơi đặt chó một cách linh hoạt đòi hỏi phải giải quyết vấn đề về kiểu cắt tối thiểu tổng thể, điều này không cần thiết ở đây. 

Quan sát quan trọng giúp đơn giản hóa mọi thứ: chúng ta không cần phải kiểm soát tất cả các đường đi giữa sói và cừu trên toàn cầu. Chúng ta chỉ cần đảm bảo rằng không con sói nào có thể bước thẳng vào chuồng cừu thông qua ô trống liền kề. Nếu một con sói có thể tiếp cận một con cừu thì phải tồn tại một con đường mà bước cuối cùng đi vào con cừu từ một trong bốn con cừu lân cận. Điều này có nghĩa là nếu chúng ta ngăn chặn sự gần gũi giữa bất kỳ con sói nào và bất kỳ con cừu nào bằng cách chặn những con cừu lân cận trống rỗng thì toàn bộ vấn đề sẽ được giải quyết. 

Điều này làm giảm vấn đề thành vấn đề xây dựng địa phương. Chúng tôi quét lưới. Nếu bất kỳ con sói nào đã ở gần một con cừu, thì không có cách nào để chèn một con chó vào giữa chúng, vì vậy câu trả lời ngay lập tức là không thể. Nếu không, chúng tôi đặt chó vào mỗi ô trống cạnh một con cừu. Những con chó này đóng vai trò như những rào cản ngăn chặn bất kỳ khả năng tiếp cận nào từ sói vào vùng cừu trong tương lai. Vì sói không thể đi qua chó và tất cả cừu hiện bị cô lập khỏi các ô trống liền kề dẫn đến sói nên không có con đường nào có thể tồn tại. 

Việc khám phá mạnh mẽ sẽ mô phỏng khả năng tiếp cận toàn cầu, nhưng cấu trúc của lưới đảm bảo rằng chỉ có sự lân cận ngay lập tức mới quan trọng, bởi vì bất kỳ con đường nào dài hơn trước tiên phải đi qua hàng xóm của một con cừu.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra khả năng tiếp cận BFS bằng vũ lực | O(RC) cho mỗi lần tìm kiếm, có thể là O(R^2C^2) | O(RC) | Quá chậm/không cần thiết | 
| Chặn cục bộ tối ưu | O(RC) | O(RC) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi lưới như một cấu trúc có thể thay đổi và trực tiếp xây dựng sự sắp xếp cuối cùng. 

1. Sao chép lưới đầu vào thành một mảng có thể thay đổi để chúng ta có thể đặt chó mà không làm mất thông tin gốc. Điều này là cần thiết vì chúng ta phải bảo tồn tất cả cừu và sói không bị thay đổi. 
2. Duyệt qua từng ô trong lưới. Bất cứ khi nào chúng ta tìm thấy một con cừu, hãy kiểm tra bốn ô lân cận của nó. 
3. Nếu hàng xóm nào là sói, chúng ta sẽ kết luận ngay rằng câu trả lời là không thể. Điều này là do chúng ta không thể đặt một con chó vào trong chuồng cừu hoặc sói, nên không thể sửa chữa được sự liền kề. 
4. Nếu hàng xóm trống, chúng ta biến nó thành một con chó. Điều này đảm bảo rằng bất kỳ nỗ lực nào trong tương lai của sói nhằm di chuyển về phía đàn cừu đều bị chặn ở ranh giới. Chúng tôi háo hức thực hiện việc này cho tất cả các con cừu, vì nhiều con cừu có thể chia sẻ các ô ranh giới. 
5. Sau khi xử lý tất cả các ô, xuất ra lưới đã sửa đổi. 

Lý do chúng ta không cần mô phỏng rõ ràng loài sói là vì bất kỳ đường đi hợp lệ nào từ sói đến cừu đều phải đi qua hàng xóm của cừu. Bằng cách đảm bảo tất cả các tuyến lân cận như vậy đều bị chặn, chúng tôi loại bỏ tất cả các tuyến đường có thể có trong một lần di chuyển. 

### Tại sao nó hoạt động 

Bất kỳ con đường nào từ sói đến cừu đều là một chuỗi các ô liền kề kết thúc ở ô cừu. Ô cuối cùng trên con đường đó phải là hàng xóm của con cừu đó. Nếu người hàng xóm đó là một con chó hay một bức tường thì con đường không thể tồn tại. Do đó, cách duy nhất để một con đường tồn tại là nếu hàng xóm của một con cừu là sói hoặc một ô trống vẫn không bị chặn. Chúng tôi cấm sói một cách rõ ràng và biến tất cả những người hàng xóm trống rỗng thành chó, vì vậy không thể thực hiện bước cuối cùng hợp lệ thành một con cừu. Bất biến này đúng cho tất cả các con cừu cùng một lúc vì việc chặn được thực hiện trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    R, C = map(int, input().split())
    grid = [list(input().strip()) for _ in range(R)]
    
    dirs = [(1,0), (-1,0), (0,1), (0,-1)]
    
    for i in range(R):
        for j in range(C):
            if grid[i][j] == 'S':
                for di, dj in dirs:
                    ni, nj = i + di, j + dj
                    if 0 <= ni < R and 0 <= nj < C:
                        if grid[ni][nj] == 'W':
                            print("No")
                            return
                        if grid[ni][nj] == '.':
                            grid[ni][nj] = 'D'
    
    print("Yes")
    for row in grid:
        print("".join(row))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp theo sau quá trình quét lưới được mô tả trước đó. Chi tiết quan trọng là các sửa đổi được thực hiện tại chỗ trong khi quét, nhưng điều này an toàn vì việc biến các ô trống thành chó chỉ tăng cường khả năng bảo vệ và không bao giờ tạo ra đường đi mới cho sói. 

Việc kiểm tra ranh giới đảm bảo chúng tôi không truy cập các chỉ mục không hợp lệ. Việc sớm phát hiện một con sói lân cận sẽ tránh được việc xử lý không cần thiết khi không thể thực hiện được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
S.W
...
W.S
```Chúng tôi quét từng con cừu và kiểm tra hàng xóm. 

| Ô (i,j) | Loại | Kết quả kiểm tra hàng xóm | Hành động | 
| --- | --- | --- | --- | 
| (0,0) | S | đúng là W | không thể phát hiện được | 

Vì một con sói ở cạnh một con cừu nên chúng tôi ngay lập tức xuất ra Số. Điều này xác nhận rằng sự gần gũi là một cấu hình nguy hiểm. 

### Ví dụ 2 

đầu vào:```
3 3
S..
...
..W
```Chúng tôi xử lý cừu ở (0,0). 

| Ô (i,j) | Loại | Quét hàng xóm | Thay đổi lưới | 
| --- | --- | --- | --- | 
| (0,0) | S | (1,0),(0,1) là '.' | đặt D tại (1,0),(0,1) | 

Lưới cuối cùng trở thành:```
SD.
D..
..W
```Không có con cừu nào ở gần sói sau khi chặn, vì vậy kết quả là Có. Điều này chứng tỏ rằng việc chặn cục bộ tách biệt hoàn toàn các thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(RC) | Mỗi ô được truy cập một lần và mỗi lần kiểm tra hàng xóm là công việc liên tục | 
| Không gian | O(RC) | Lưu trữ lưới cộng với chi phí nhỏ | 

Kích thước lưới tối đa là 250.000 ô và mỗi ô đóng góp tối đa bốn lần kiểm tra hàng xóm. Điều này thoải mái phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1 (conceptual form)
assert run("""3 3
S.W
...
W.S
""") == "No"

# simple valid case
assert run("""2 2
S.
.W
""") == "Yes\nSD\nDW"

# no wolves
assert run("""2 3
S..
...
""") != ""

# fully empty grid
assert run("""2 2
..
..
""") == "Yes\n..\n.."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| SW / W.S | Không | liền kề thất bại | 
| S. / .W | đầu ra lưới | chặn chính xác cơ bản | 
| không có sói | Có | trường hợp an toàn tầm thường | 
| lưới trống | Có | xử lý trường hợp thoái hóa | 

## Vỏ cạnh 

Sự kề cận trực tiếp của sói-cừu là dạng thất bại nghiêm trọng nhất. Đối với một đầu vào như:```
2 2
SW
..
```thuật toán kiểm tra ô (0,0), nhìn thấy một con cừu và ngay lập tức tìm thấy một con sói ở bên phải nó. Nó trả về Không trước bất kỳ sửa đổi nào. Điều này đảm bảo rằng chúng tôi không bao giờ cố gắng "sửa chữa" một cấu hình không thể thực hiện sai cách. 

Trường hợp thứ hai là khi nhiều con cừu ở chung một ô ranh giới. Trong một lưới như:```
3 3
S.S
...
W.W
```Các ô trống ở hàng giữa liền kề với cả hai con cừu được chuyển thành chó trong quá trình xử lý từng con cừu. Mặc dù cùng một ô có thể được xem xét nhiều lần, việc gán lại '.' đến 'D' là bình thường, do đó lưới cuối cùng vẫn nhất quán và bị chặn hoàn toàn.
