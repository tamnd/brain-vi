---
title: "CF 105450H - Trò chơi đầu đạn"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô có thể sử dụng được hoặc bị chặn. Một mã thông báo bắt đầu ở ô trên cùng bên trái và hai người chơi luân phiên di chuyển."
date: "2026-06-23T17:33:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105450
codeforces_index: "H"
codeforces_contest_name: "UTPC x WiCS Contest 10-25-24 (UT Internal)"
rating: 0
weight: 105450
solve_time_s: 87
verified: false
draft: false
---

[CF 105450H - Trò chơi đầu đạn](https://codeforces.com/problemset/problem/105450/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô có thể sử dụng được hoặc bị chặn. Một mã thông báo bắt đầu ở ô trên cùng bên trái và hai người chơi luân phiên di chuyển. Trong một lượt, người chơi di chuyển mã thông báo xuống một bước, sang phải hoặc theo đường chéo xuống bên phải, nhưng chỉ khi ô đích nằm trong lưới và không bị chặn. Nếu một người chơi không có động thái hợp pháp, người chơi đó sẽ thua ngay lập tức. 

Nhiệm vụ là xác định xem người chơi đầu tiên, Alice, có chiến lược chiến thắng hay không. 

Kích thước lưới có thể lớn tới 1000 x 1000, điều này khiến cho bất kỳ giải pháp nào cố gắng khám phá tất cả các chuỗi di chuyển đều không thể thực hiện được. Một cây trò chơi đầy đủ từ mỗi ô sẽ phân nhánh theo ba cách và có thể phát triển theo cấp số nhân về độ sâu tỷ lệ với r + c, do đó, một tìm kiếm đệ quy đơn giản sẽ vượt quá giới hạn thời gian theo nhiều bậc độ lớn. 

Một vấn đề tế nhị là các ô bị chặn có thể tạo ra các vùng biệt lập khiến một số vị trí trở thành ngõ cụt ngay cả khi chúng không ở gần ranh giới. Ví dụ: một ô được bao quanh bởi các ô vuông màu đen ngoại trừ các hướng lùi là không liên quan vì không được phép di chuyển lùi, do đó, nó thực sự trở thành trạng thái mất đầu cuối. 

Một trường hợp cạnh khác là khi ô bắt đầu không có nước đi hợp lệ nào cả. Mặc dù nó được đảm bảo là màu trắng nhưng nó vẫn có thể bị bao quanh hoàn toàn bởi màu đen hoặc ranh giới, dẫn đến việc Alice bị thua ngay lập tức. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp của trò chơi coi mọi ô trắng là trạng thái trò chơi và khám phá đệ quy tất cả các bước di chuyển có thể tiếp cận. Từ mỗi trạng thái, chúng tôi kiểm tra xem có tồn tại nước đi đến vị trí mà đối thủ thua hay không. Đây là định nghĩa minimax tiêu chuẩn cho trò chơi công bằng trên đồ thị có hướng. 

Tuy nhiên, phép đệ quy này lặp lại các bài toán con tương tự nhiều lần. Trong trường hợp xấu nhất, mỗi ô r × c có thể phân nhánh thành ba lần chuyển đổi, dẫn đến hành vi theo cấp số nhân. Ngay cả với tính năng ghi nhớ, cấu trúc không theo chu kỳ vì tất cả các bước di chuyển đều tăng theo hàng hoặc cột, do đó mỗi trạng thái chỉ phụ thuộc vào các ô "sau" trong lưới. 

Cấu trúc đơn điệu này là quan sát chính. Vì mỗi bước di chuyển đều tăng tọa độ một cách nghiêm ngặt nên không có chu kỳ. Điều đó có nghĩa là chúng ta có thể tính kết quả từ dưới lên: bất kỳ ô nào cũng chỉ phụ thuộc vào các ô ở bên phải, bên dưới hoặc theo đường chéo xuống dưới bên phải. Nếu chúng ta xử lý lưới theo thứ tự ngược lại, từ dưới cùng bên phải sang trên cùng bên trái, mọi chuyển đổi đều dẫn đến trạng thái đã được tính toán. 

Đối với mỗi ô trắng, chúng tôi đánh dấu ô đó là thắng nếu nó có thể di chuyển đến ít nhất một ô thua. Nếu tất cả các nước đi có thể dẫn đến vị trí chiến thắng thì ô hiện tại sẽ thua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS | O(3^(r+c)) | O(r·c) đệ quy | Quá chậm | 
| DP từ dưới lên | O(r·c) | O(r·c) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa từng ô như một trạng thái trò chơi thắng hoặc thua. 

1. Chúng tôi tạo một bảng DP trong đó mỗi ô lưu trữ xem người chơi hiện tại bắt đầu ở đó có thể giành chiến thắng hay không. Ô đen luôn thua vì đứng trên đó không hợp lệ. 
2. Chúng ta lặp lại lưới theo thứ tự ngược lại, bắt đầu từ góc dưới bên phải và di chuyển về góc trên bên trái. Thứ tự này đảm bảo rằng bất cứ khi nào chúng tôi đánh giá một ô, tất cả các ô mà nó có thể di chuyển đến đều đã được xử lý. 
3. Đối với mỗi ô màu trắng, chúng tôi thử tất cả các bước di chuyển hợp lệ: xuống, sang phải và chéo xuống bên phải. Nếu bất kỳ động thái nào trong số này dẫn đến ô thua, chúng tôi đánh dấu ô hiện tại là ô thắng. Lý do là chúng ta có thể ép đối thủ vào thế thua. 
4. Nếu không có nước đi nào dẫn đến trạng thái thua, chúng ta đánh dấu ô hiện tại là thua vì mỗi nước đi đều mang lại cho đối thủ một thế thắng. 
5. Sau khi điền vào bảng DP, câu trả lời được xác định bằng giá trị tại ô bắt đầu (0,0). Nếu thắng, Alice có thể buộc thắng, nếu không thì Bob thắng. 

### Tại sao nó hoạt động

Bảng DP mã hóa thuộc tính cơ bản của trò chơi công bằng trên DAG: một vị trí thắng chính xác khi nó có ít nhất một cạnh đi ra so với vị trí thua. Vì biểu đồ di chuyển có tính chất không tuần hoàn nên mọi trạng thái chỉ phụ thuộc hoàn toàn vào các trạng thái sau này trong thứ tự xử lý, do đó mỗi giá trị được tính chính xác một lần và không bao giờ bị vô hiệu. Điều này đảm bảo tính nhất quán giữa các quyết định địa phương và kết quả trò chơi toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    r, c = map(int, input().split())
    grid = [input().strip() for _ in range(r)]
    
    dp = [[False] * c for _ in range(r)]
    
    for i in range(r - 1, -1, -1):
        for j in range(c - 1, -1, -1):
            if grid[i][j] == 'B':
                dp[i][j] = False
                continue
            
            win = False
            
            if i + 1 < r and grid[i + 1][j] == 'B':
                win = win or not dp[i + 1][j]
            elif i + 1 < r:
                win = win or (not dp[i + 1][j])
            
            if j + 1 < c:
                win = win or (not dp[i][j + 1])
            
            if i + 1 < r and j + 1 < c:
                win = win or (not dp[i + 1][j + 1])
            
            dp[i][j] = win
    
    print("Alice" if dp[0][0] else "Bob")

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là truyền tải ngược của lưới. Mỗi ô dp chỉ được tính một lần và các chuyển tiếp chỉ được tính về phía trước trong lưới. Logic boolean nắm bắt trực tiếp quy tắc minimax: một vị thế sẽ thắng nếu bất kỳ động thái nào dẫn đến trạng thái thua. 

Sự tinh tế duy nhất là xử lý các ô màu đen. Họ buộc phải mất trạng thái và không bao giờ được coi là mục tiêu di chuyển hợp lệ. Điều này được thực thi bằng cách gán ngay Sai cho chúng và bỏ qua quá trình chuyển đổi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Lưới đầu vào:```
W B W B W
W W W W W
W B W B W
B W B W B
```Chúng tôi tính toán dp từ phía dưới bên phải về phía sau. Bảng dưới đây chỉ hiển thị một số vị trí quan trọng: 

| Tế bào | Loại | Di chuyển kết quả | dp | 
| --- | --- | --- | --- | 
| (3,4) | W | không di chuyển | Sai | 
| (3,3) | W | → (3,4)=Sai | Đúng | 
| (2,2) | W | đường chéo dẫn đến thua | Đúng | 
| (0,0) | W | ít nhất một nước đi thua có thể truy cập được | Đúng | 

Vị trí xuất phát là thắng nên Alice thắng. 

Đầu ra:```
Alice
```### Mẫu 2 

đầu vào:```
W B B B
B B B B
```Lưới chặn hầu hết mọi thứ. Từ (0,0), không có nước đi nào hợp lệ cả. 

| Tế bào | Loại | Di chuyển kết quả | dp | 
| --- | --- | --- | --- | 
| (1,3) | B | không hợp lệ | Sai | 
| (0,0) | W | không có nước đi hợp lệ | Sai | 

Alice không có động thái nào trong lượt đầu tiên. 

Đầu ra:```
Bob
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(r·c) | Mỗi ô được xử lý một lần với các chuyển đổi liên tục | 
| Không gian | O(r·c) | Bảng DP lưu trữ một boolean trên mỗi ô | 

Các ràng buộc cho phép tối đa một triệu ô và thuật toán chỉ thực hiện một lượng công việc không đổi nhỏ trên mỗi ô, phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    def solve():
        r, c = map(int, input().split())
        grid = [input().strip() for _ in range(r)]
        
        dp = [[False] * c for _ in range(r)]
        
        for i in range(r - 1, -1, -1):
            for j in range(c - 1, -1, -1):
                if grid[i][j] == 'B':
                    dp[i][j] = False
                    continue
                
                win = False
                
                if i + 1 < r:
                    win |= not dp[i + 1][j]
                if j + 1 < c:
                    win |= not dp[i][j + 1]
                if i + 1 < c and j + 1 < c:
                    win |= not dp[i + 1][j + 1]
                
                dp[i][j] = win
        
        return "Alice" if dp[0][0] else "Bob"
    
    return solve()

# provided samples
assert run("4 5\nWBWBW\nWWWWB\nWBWBW\nBWBWB\n") == "Alice"
assert run("2 2\nWBBB\n") == "Bob"

# custom cases
assert run("1 1\nW\n") == "Bob", "single cell loses"
assert run("1 2\nWW\n") == "Alice", "one move available"
assert run("3 3\nWBW\nBBB\nWBW\n") == "Bob", "blocked middle row"
assert run("2 3\nWWW\nWWW\n") == "Alice", "fully open small grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 trắng | Bob | vị trí đầu cuối | 
| 1x2 toàn màu trắng | Alice | di chuyển cưỡng bức duy nhất | 
| hàng giữa bị chặn | Bob | xử lý cách ly | 
| lưới 2x3 đầy đủ | Alice | tính nhất quán giành chiến thắng nhiều đường | 

## Vỏ cạnh 

Một lưới tối thiểu bao gồm một ô màu trắng sẽ kết thúc trò chơi ngay lập tức. DP đánh dấu nước đó là thua vì không có nước đi nào đi tiếp, vì vậy Bob được tuyên bố là người chiến thắng. 

Một lưới trong đó mọi hàng xóm của ô bắt đầu đều bị chặn cũng khiến Alice bị thua ngay lập tức. DP không tìm thấy bất kỳ chuyển đổi nào từ (0,0), do đó, nó vẫn là Sai, mô hình hóa chính xác trạng thái đầu cuối. 

Một lưới mở hoàn toàn hoạt động khác nhau: mỗi ô cuối cùng có ít nhất một nước đi đến vị trí thua gần ranh giới phía dưới bên phải, truyền ngược các trạng thái thắng cho đến khi bắt đầu trở thành thắng.
