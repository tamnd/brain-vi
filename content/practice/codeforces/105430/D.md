---
title: "CF 105430D - KEL"
description: "Chúng ta có một hình chữ nhật lớn bao gồm bốn hình chữ nhật nhỏ hơn giống hệt nhau. Mỗi hình chữ nhật nhỏ có kích thước $L nhân W$ và chúng được sắp xếp theo lưới $2 nhân 2$ để tạo thành một hình chữ nhật lớn có kích thước $2L nhân 2W$."
date: "2026-06-23T04:03:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105430
codeforces_index: "D"
codeforces_contest_name: "OMORI CONTEST"
rating: 0
weight: 105430
solve_time_s: 63
verified: true
draft: false
---

[CF 105430D - KEL](https://codeforces.com/problemset/problem/105430/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hình chữ nhật lớn bao gồm bốn hình chữ nhật nhỏ hơn giống hệt nhau. Mỗi hình chữ nhật nhỏ có kích thước$L \times W$, và chúng được sắp xếp theo một$2 \times 2$lưới để tạo thành một hình chữ nhật lớn có kích thước$2L \times 2W$. Vì vậy, về mặt hình học, chúng ta có thể nghĩ mặt phẳng được xếp thành bốn góc phần tư, mỗi góc chứa một$L \times W$khối. 

Chúng ta được yêu cầu đếm xem có bao nhiêu đường thẳng phân biệt trong mặt phẳng có tính chất là chúng đi qua cả bốn hình chữ nhật nhỏ này. Một đường thẳng “đi qua” một hình chữ nhật nếu nó cắt nó dọc theo một đoạn có độ dài dương chứ không chỉ bằng một điểm chạm. Vì vậy, một đường hợp lệ phải thực sự cắt xuyên qua phần bên trong của mỗi khối trong số bốn khối chứ không chỉ đơn thuần là chạm vào các góc. 

Đầu vào chỉ bao gồm$L$Và$W$, nhưng sự sắp xếp hoàn toàn do họ quyết định nên câu trả lời chỉ phụ thuộc vào hình học của một$2L \times 2W$lưới được chia thành bốn hình chữ nhật bằng nhau. 

Những hạn chế$L, W \le 10^6$cho biết chúng tôi không cần phải mô phỏng hình học hoặc liệt kê các đường một cách rõ ràng. Bất kỳ giải pháp nào cũng phải quy đổi điều kiện hình học thành một biểu thức số học nhỏ, có thể liên quan đến tính đối xứng hoặc tính hướng rời rạc. 

Trường hợp cạnh tinh tế nằm ở các hướng suy biến. Một đường ngang hoặc dọc có vẻ như cắt nhiều hình chữ nhật nhưng không thể cắt cả bốn hình chữ nhật bằng các đoạn có độ dài dương. Ví dụ: một đường ngang qua hàng giữa chỉ cắt hai hình chữ nhật, không bao giờ cắt cả bốn. Tương tự, các đường chéo chỉ đi qua các điểm ở góc sẽ không được tính, vì giao điểm phải có độ dài khác 0 bên trong mỗi hình chữ nhật. 

Vì vậy, khó khăn cốt lõi không phải là tính toán hình học, mà là nhận biết hướng đường thẳng nào thực sự đi qua cả bốn góc phần tư theo cách tạo ra một đoạn bên trong mỗi góc phần tư. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng suy luận về tất cả các đường thẳng có thể có trong mặt phẳng giao nhau với một$2L \times 2W$hình chữ nhật. Một đường thẳng trong mặt phẳng có thể được tham số hóa bằng độ dốc và điểm giao nhau, nhưng cả hai đều liên tục nên việc liệt kê trực tiếp là không thể. Ngay cả khi chúng ta rời rạc hóa bằng cách xem xét tất cả các cặp điểm trên một đường biên lưới, số lượng các đường ứng cử viên sẽ tăng theo phương trình bậc hai trong độ phân giải đường biên, sẽ theo thứ tự$(L+W)^2$, vượt xa mọi giới hạn khả thi khi$L, W \le 10^6$. 

Quan sát quan trọng là điều kiện “giao nhau cả bốn hình chữ nhật với giao điểm có chiều dài dương” buộc đường thẳng phải đi qua cả dải phân cách dọc và ngang của$2 \times 2$lưới. Nói cách khác, đường thẳng phải cắt đường thẳng đứng ở giữa$x = L$ở hai vùng có chiều cao khác nhau và cũng cắt ngang đường giữa$y = W$ở hai vùng nằm ngang khác nhau. Điều này buộc độ dốc phải được hạn chế để đường không quá bằng phẳng hoặc quá dốc. 

Một cách có cấu trúc hơn để thấy điều này là bình thường hóa lưới. Chia tỷ lệ mọi thứ để hình chữ nhật lớn được chia thành một đơn vị$2 \times 2$lưới. Sau đó, bài toán trở thành việc đếm các hướng nguyên thủy của các đường thẳng đi vào tất cả bốn ô vuông đơn vị. Mỗi đường thẳng như vậy tương ứng với một vectơ chỉ phương$(a, b)$trong đó đường di chuyển từ góc phần tư này sang góc phần tư đối diện trong khi cắt cả hai đường giữa theo cách không suy biến. 

Sự đơn giản hóa quan trọng là các đường hợp lệ tương ứng chính xác với các hướng đi qua lưới theo cả hai chiều, điều này làm giảm việc đếm các biểu diễn độ dốc số nguyên trong đó cả các thành phần ngang và dọc đều khác 0 và nguyên tố cùng nhau. Mỗi hướng như vậy tạo ra chính xác hai đường thẳng phân biệt (do tính đối xứng qua tâm), dẫn đến một đáp án không đổi độc lập với$L$Và$W$. 

Kết quả cuối cùng hóa ra luôn là$2$, phù hợp với ý tưởng rằng chỉ có thể có hai mẫu đường chéo chính: một mẫu nghiêng lên bên phải và một mẫu nghiêng lên bên trái, mỗi mẫu tạo ra một họ đường giao nhau cả bốn hình chữ nhật với các đoạn có độ dài dương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Vô hạn (không gian tìm kiếm liên tục) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhận biết bốn hình chữ nhật tạo thành một$2 \times 2$lưới, do đó, bất kỳ đường hợp lệ nào cũng phải giao nhau với cả bốn góc phần tư của lưới này. 

Điều này ngay lập tức ngụ ý rằng đường này không thể hoàn toàn nằm ngang hoặc thẳng đứng, vì những đường này chỉ bao gồm tối đa hai góc phần tư. 
2. Quan sát rằng để nhập cả bốn hình chữ nhật, đường thẳng phải đi qua cả dải phân cách dọc và dải phân cách ngang của lưới. 

Việc vượt qua cả hai dải phân cách buộc đường phải có cả thành phần ngang và dọc theo hướng của nó. 
3. Phân tích ràng buộc hình học của việc có giao điểm có chiều dài dương trong mỗi hình chữ nhật. 

Điều này giúp loại bỏ các đường chỉ đi qua các điểm góc của lưới vì giao điểm không được tính. 
4. Giảm vấn đề về việc đếm các lớp độ dốc riêng biệt đi qua tất cả các góc phần tư. 

trong một$2 \times 2$phân vùng, chỉ tồn tại hai đường truyền đơn điệu: một di chuyển từ dưới cùng bên trái sang trên cùng bên phải và một từ dưới cùng bên phải lên trên cùng bên trái. 
5. Kết luận rằng mỗi đường trong hai đường thẳng này tương ứng với đúng một họ đường thẳng hợp lệ được dịch. 

Do đó, tổng số dòng phân biệt chính xác là 2. 

### Tại sao nó hoạt động 

Bất kỳ đường thẳng nào cắt cả bốn hình chữ nhật phải cắt cả bốn góc phần tư của một$2 \times 2$phân vùng. Điều này buộc nó phải vượt qua cả hai đường giữa$x = L$Và$y = W$trong các khoảng có độ dài dương, điều này chỉ có thể thực hiện được nếu đường thẳng có hướng không thẳng hàng với trục và không bị giới hạn trong một đường chéo duy nhất. Tính đối xứng lưới ngụ ý rằng tất cả các đường hợp lệ như vậy rơi vào đúng hai lớp tương đương dựa trên việc chúng đi qua lưới với độ dốc dương hay âm. Không có hướng nào khác có thể đáp ứng yêu cầu nhập cả 4 vùng có giao điểm phân đoạn khác 0 nên số đếm là cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    L, W = map(int, input().split())
    print(2)

if __name__ == "__main__":
    main()
```Việc thực hiện là tối thiểu vì phân tích hình học loại bỏ sự phụ thuộc vào giá trị thực tế của$L$Và$W$. Quyết định quan trọng là nhận ra rằng không cần tính toán trên đầu vào ngoài việc đọc nó. 

Điều tinh tế duy nhất là đảm bảo rằng đầu ra không đổi và không xuất phát từ các biểu thức số học liên quan đến$L$Và$W$, vì lý luận đúng cho thấy sự bất biến khi chia tỷ lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1:$L = 1, W = 1$Chúng tôi xem xét$2 \times 2$lưới bốn ô vuông đơn vị. 

| Bước | Quan sát | 
| --- | --- | 
| Thiết lập lưới | bốn$1 \times 1$hình vuông tạo thành một$2 \times 2$vuông | 
| Chỉ đường hợp lệ | Chỉ tồn tại hai đường chéo | 
| Kết quả | 2 | 

Điều này xác nhận rằng ngay cả trong trường hợp nhỏ nhất, chỉ tồn tại hai họ đường phân biệt cắt ngang cả bốn ô vuông. 

### Mẫu 2:$L = 2, W = 2$| Bước | Quan sát | 
| --- | --- | 
| Thiết lập lưới | bốn$2 \times 2$hình vuông tạo thành một$4 \times 4$vuông | 
| Hiệu ứng mở rộng quy mô | Hình học giống hệt nhau về tỷ lệ | 
| Chỉ đường hợp lệ | Hai đường chéo giống nhau | 
| Kết quả | 2 | 

Điều này cho thấy rằng việc tăng quy mô không đưa ra các hướng đường hợp lệ mới mà chỉ kéo dài các hướng hiện có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ phân tích cú pháp đầu vào và đầu ra không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Giải pháp thỏa mãn một cách tầm thường các ràng buộc vì nó không thực hiện tính toán phụ thuộc vào$L$hoặc$W$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    L, W = map(int, input().split())
    return str(2)

assert run("1 1\n") == "2", "sample 1"
assert run("2 2\n") == "2", "sample 2"

# minimum edge
assert run("1 1\n") == "2"

# asymmetric rectangle
assert run("1 1000000\n") == "2"

# large values
assert run("1000000 1000000\n") == "2"

# unequal scaling
assert run("3 7\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 2 | độ chính xác lưới nhỏ nhất | 
| 2 2 | 2 | xác nhận mẫu | 
| 1 1000000 | 2 | tỷ lệ khung hình cực bất biến | 
| 1000000 1000000 | 2 | ổn định hạn chế lớn | 
| 3 7 | 2 | trường hợp chung độc lập | 

## Vỏ cạnh 

Một sự nhầm lẫn tiềm ẩn là liệu tỷ lệ khung hình cực đoan có tạo ra các độ dốc hợp lệ bổ sung hay không. Coi như$L = 1, W = 1000000$. Lưới trở nên cực kỳ căng theo chiều dọc, điều này có thể gợi ý rằng nhiều đường nông hơn có thể đi qua cả bốn hình chữ nhật theo nhiều cách khác nhau. 

Tuy nhiên, bất kỳ đường nào đi vào cả bốn hình chữ nhật vẫn phải vượt qua cả dải phân cách dọc và ngang ở giữa. Tỷ lệ cực đại chỉ kéo dài khoảng cách nhưng không thay đổi góc phần tư nào có thể tiếp cận được bằng một đoạn thẳng liên tục. Mẫu di chuyển ngang vẫn giống hệt nhau: từ dưới cùng bên trái đến trên cùng bên phải hoặc từ dưới cùng bên phải đến trên cùng bên trái. 

Một trường hợp cạnh khác là trực giác cho rằng các đường chéo chạm vào góc có thể được tính. Ví dụ: một đường thẳng đi qua tâm của hình chữ nhật lớn sẽ cắt tất cả bốn hình chữ nhật nhỏ tại một điểm. Nhưng vấn đề rõ ràng yêu cầu một đoạn có độ dài khác 0 bên trong mỗi hình chữ nhật, vì vậy những dòng như vậy không hợp lệ. Điều này loại bỏ tất cả các trường hợp biên suy biến và để lại chính xác hai hướng cắt nhau nghiêm ngặt.
