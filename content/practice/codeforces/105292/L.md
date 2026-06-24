---
title: "CF 105292L - Trò chơi cờ bàn của Ltf"
description: "Chúng ta được cung cấp một lưới $N nhân N$ trong đó hai người chơi lần lượt đặt các mã thông báo vào các ô trống. Hạn chế mang tính toàn cầu: không có hai mã thông báo được đặt nào được phép liền kề trực giao, nghĩa là chúng không thể chia sẻ một bên."
date: "2026-06-24T20:16:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105292
codeforces_index: "L"
codeforces_contest_name: "National Taiwan University Class Preliminary 2024"
rating: 0
weight: 105292
solve_time_s: 51
verified: true
draft: false
---

[CF 105292L - Trò chơi cờ bàn của Ltf](https://codeforces.com/problemset/problem/105292/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$N \times N$lưới nơi hai người chơi luân phiên đặt mã thông báo vào các ô trống. Hạn chế mang tính toàn cầu: không có hai mã thông báo được đặt nào được phép liền kề trực giao, nghĩa là chúng không thể chia sẻ một bên. Khi một ô đã bị chiếm giữ, tất cả bốn ô lân cận cạnh của nó sẽ bị hạn chế đối với các vị trí trong tương lai trừ khi chúng đã bị chiếm giữ, vì việc đặt ở đó sẽ vi phạm quy tắc. 

Trò chơi kết thúc khi một người chơi không có động thái hợp pháp trong lượt của mình và người chơi đó thua. Ltf thực hiện nước đi đầu tiên và cả hai người chơi đều được cho là đã chơi hoàn hảo. Nhiệm vụ không phải là mô phỏng trò chơi mà là xác định kích thước bảng nhất định$N$, liệu người chơi đầu tiên có thể giành chiến thắng hay không. 

Đầu vào là một số nguyên duy nhất$N$và đầu ra là một từ duy nhất cho biết người chiến thắng. 

Mặc dù lưới có thể lớn như$10^5 \times 10^5$, các quy tắc chỉ phụ thuộc vào sự liền kề chứ không phụ thuộc vào bất kỳ cấu trúc động nào phát triển trong quá trình chơi. Điều này gợi ý rõ ràng rằng câu trả lời phụ thuộc vào sự bất biến về cấu trúc của lưới hơn là bất kỳ quá trình mô phỏng nào. 

Một cách giải thích ngây thơ sẽ cố gắng mô hình hóa trạng thái trò chơi dưới dạng một biểu đồ trong đó mỗi nước đi sẽ loại bỏ một đỉnh và cấm các đỉnh lân cận của nó. Điều đó ngay lập tức dẫn đến một trò chơi tổ hợp trên biểu đồ lưới với sự phân nhánh theo cấp số nhân. Ngay cả đối với nhỏ$N$, việc liệt kê tất cả các chuỗi nước đi trở nên không khả thi vì mỗi nước đi ảnh hưởng đến tối đa bốn nước láng giềng và cây trò chơi phát triển cực kỳ nhanh chóng. 

Trường hợp cạnh dễ nhìn thấy nhất trên các lưới nhỏ. 

Vì$N = 1$, có đúng một ô. Người chơi đầu tiên lấy nó, người thứ hai không di chuyển nên người đầu tiên thắng. 

Vì$N = 2$, bất kỳ nước đi nào cũng sẽ chặn một phần đáng kể của bàn cờ. Trên thực tế, sau vị trí đầu tiên, không thể thực hiện nước đi thứ hai, vì vậy người chơi đầu tiên sẽ thua nếu lối chơi tối ưu dẫn đến bế tắc ngay lập tức sau khi đưa ra cho đối thủ một chuỗi nước đi bắt buộc dẫn đến cấu trúc trả lời thắng. Mẫu chính thức xác nhận rằng$N=2$đang thua người chơi đầu tiên. 

Vì$N = 3$, có đủ không gian để sắp xếp xen kẽ các vị trí sao cho người chơi đầu tiên luôn có thể phản chiếu hoặc kiểm soát tính chẵn lẻ của các ô còn lại có thể sử dụng được và người chơi đầu tiên sẽ thắng. 

Những ví dụ này đã gợi ý rằng có liên quan đến cấu trúc chẵn lẻ hoặc lưỡng cực chứ không phải là độ phức tạp hình học. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa mọi cấu hình vị trí hợp lệ dưới dạng trạng thái trò chơi và mô phỏng đệ quy tất cả các bước di chuyển có thể xảy ra, đánh dấu các trạng thái là thắng hoặc thua bằng cách sử dụng minimax. Một bước di chuyển bao gồm việc chọn một ô không liền kề với bất kỳ ô nào đã được chọn, sau đó cập nhật vùng cấm. Trong trường hợp xấu nhất, số lượng cấu hình hợp lệ của một$N \times N$lưới dưới một ràng buộc độc lập là theo cấp số nhân trong$N^2$, vì vậy việc khám phá không gian trạng thái này là hoàn toàn không khả thi ngay cả đối với$N=10$. 

Quan sát quan trọng là hạn chế kề cận biến lưới thành biểu đồ hai bên trong đó mọi ô đều có màu giống như bàn cờ. Bất kỳ tập hợp các mảnh được đặt hợp lệ nào cũng phải tạo thành một tập hợp độc lập trong biểu đồ này. Trò chơi khi đó tương đương với việc người chơi luân phiên chọn các đỉnh sao cho không có hai đỉnh được chọn nào liền kề với các đỉnh đã chọn, điều này tương đương với việc từng bước xây dựng một tập hợp độc lập tối đa. 

Trên biểu đồ hai bên, cấu trúc chơi tối ưu thu gọn thành một đối số chẵn lẻ đơn giản: người chơi đầu tiên thắng khi và chỉ khi tổng số “cơ hội ghép đôi bắt buộc” có sẵn không vô hiệu hóa lợi thế nước đi đầu tiên. Đối với một lưới đầy đủ không có ô bị chặn, kết quả chỉ phụ thuộc vào việc kích thước lưới dẫn đến cấu trúc phân vùng cân bằng hay không cân bằng. 

Đối với một$N \times N$lưới, cả hai lớp màu của màu bàn cờ có kích thước bằng nhau khi$N$là số chẵn và khác nhau đúng một khi$N$thật kỳ quặc. Sự mất cân bằng này là yếu tố quyết định ai là người có nước đi cuối cùng trong lối chơi tối ưu. Giới hạn lân cận đảm bảo rằng mọi di chuyển đều tiêu tốn một cách hiệu quả một ô từ cấu trúc lưỡng cực này mà không cho phép tương tác chéo giữa các lựa chọn cùng màu. 

Điều này làm giảm toàn bộ trò chơi để kiểm tra tính chẵn lẻ của$N$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng trò chơi Brute Force | số mũ trong$N^2$| Hàm mũ | Quá chậm | 
| Giảm cân bằng lưỡng cực |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng lưới có thể được tô màu giống như một bàn cờ, chia tất cả các ô thành hai tập hợp riêng biệt trong đó không có hai ô nào trong một tập hợp liền kề nhau. Điều này chuyển đổi quy tắc vị trí thành việc chọn các đỉnh độc lập trong biểu đồ hai bên. 
2. Nhận biết rằng mỗi bước di chuyển sẽ loại bỏ chính xác một ô khỏi tập hợp các bước di chuyển có sẵn và có khả năng vô hiệu hóa tối đa bốn ô lân cận, nhưng những ô lân cận đó thuộc lớp màu đối lập trong cấu trúc lưỡng cực. 
3. Nhận ra rằng cấu trúc tổng thể duy nhất ảnh hưởng đến cách chơi tối ưu là sự mất cân bằng giữa hai lớp màu. Trên một$N \times N$lưới, số lượng bằng nhau khi$N$là số chẵn và khác nhau một khi$N$thật kỳ quặc. 
4. Giảm kết quả trò chơi thành quyết định ngang bằng: nếu$N$chẵn, tính đối xứng cho phép người chơi thứ hai luôn phản ứng theo cách duy trì sự cân bằng; nếu như$N$lẻ, người chơi đầu tiên được thừa hưởng ô phụ và duy trì lợi thế nước đi cuối cùng. 
5. Xuất ra kết quả thắng cuộc chỉ dựa trên điều kiện chẵn lẻ này. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau bất kỳ chuỗi nước đi hợp lệ nào, cấu trúc còn lại có thể chơi được vẫn có thể được hiểu là biểu đồ hai bên trong đó các nước đi luôn tiêu tốn một đỉnh và gián tiếp chỉ ràng buộc các đỉnh trong phân vùng đối diện. Điều này ngăn cản bất kỳ người chơi nào phá vỡ tính đối xứng tổng thể giữa hai phân vùng ngoại trừ sự mất cân bằng ban đầu do kích thước lưới gây ra. Vì lối chơi tối ưu luôn bảo toàn tính đối xứng bất cứ khi nào có thể nên kết quả chỉ phụ thuộc vào việc tính đối xứng đó có hoàn hảo hay không (thậm chí$N$) hoặc hơi thiên vị (lẻ$N$). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

# For this game, parity determines the winner
# odd N -> first player wins, even N -> second player wins
if n % 2 == 1:
    print("Ltf")
else:
    print("Ian")
```Mã đọc số nguyên duy nhất và áp dụng trực tiếp quy tắc chẵn lẻ. Không cần mô phỏng hoặc tiền xử lý vì cấu trúc trò chơi chuyển sang quyết định theo thời gian không đổi. 

Điều tinh tế duy nhất là đảm bảo xử lý chính xác định dạng đầu vào và loại bỏ khoảng trắng, vì toàn bộ logic phụ thuộc vào việc đọc một số nguyên duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```Chúng tôi bắt đầu với một lưới có kích thước chẵn. 

| Bước | Tiểu bang | Quan sát | 
| --- | --- | --- | 
| 1 | N = 2 | Lưới đối xứng hoàn hảo trong tô màu bàn cờ | 
| 2 | Quyết định | Trường hợp chẵn có nghĩa là người chơi thứ hai có thể phản chiếu nước đi | 

Đầu ra là:```
Ian
```Điều này khẳng định tính đối xứng hoàn toàn cân bằng và người chơi đầu tiên không thể tạo ra lợi thế lâu dài. 

### Ví dụ 2 

đầu vào:```
3
```Bây giờ lưới có kích thước lẻ. 

| Bước | Tiểu bang | Quan sát | 
| --- | --- | --- | 
| 1 | N = 3 | Một lớp màu có thêm một ô | 
| 2 | Quyết định | Người chơi đầu tiên có thể yêu cầu sự mất cân bằng | 

Đầu ra là:```
Ltf
```Điều này cho thấy ô phụ phá vỡ tính đối xứng và đảm bảo nước đi cuối cùng cho người chơi đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ kiểm tra tính chẵn lẻ của$N$| 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung | 

Ràng buộc đầu vào tăng lên$10^5$, nhưng vì tính toán có thời gian không đổi nên nghiệm dễ dàng nằm trong mọi giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n = int(input().strip())
    return "Ltf" if n % 2 == 1 else "Ian"

# provided samples
assert run("2\n") == "Ian"
assert run("3\n") == "Ltf"

# custom cases
assert run("1\n") == "Ltf"          # minimum case
assert run("4\n") == "Ian"          # even small grid
assert run("5\n") == "Ltf"          # odd mid case
assert run("100000\n") == "Ian"     # large even case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | Ltf | hành vi bảng nhỏ nhất | 
| 4 | Ian | trường hợp đối xứng chẵn | 
| 5 | Ltf | tính nhất quán chẵn lẻ lẻ | 
| 100000 | Ian | trường hợp ứng suất giới hạn trên | 

## Vỏ cạnh 

cho$N=1$, bảng chứa đúng một ô. Người chơi đầu tiên thực hiện ngay lập tức, không để lại nước đi hợp pháp nào cho đối thủ nên kết quả là Ltf. 

Vì$N=2$, lưới đủ nhỏ để bất kỳ hành động di chuyển nào cũng hạn chế đáng kể không gian còn lại và tính đối xứng hoàn toàn có lợi cho người chơi thứ hai. Thuật toán phân loại chính xác nó thành chẵn, tạo ra Ian. 

Đối với các giá trị chẵn lớn như$N=10^5$, cấu trúc không thay đổi theo tỷ lệ. Kiểm tra tính chẵn lẻ vẫn trả về Ian và không phát sinh vấn đề tràn hoặc hiệu suất do chỉ thực hiện một thao tác modulo duy nhất.
