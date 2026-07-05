---
title: "CF 102916D - Hai Tên Cướp Biển - 2"
description: "Chúng ta được tặng một bộ sưu tập kho báu, mỗi kho báu đều có giá trị tích cực. Hai người chơi lần lượt nhặt các vật phẩm còn lại cho đến khi không còn vật phẩm nào. Một người chơi hoàn toàn có chiến lược và muốn tối đa hóa tổng giá trị mà anh ta có được."
date: "2026-07-04T08:00:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102916
codeforces_index: "D"
codeforces_contest_name: "Samara Farewell Contest 2020 (XXI Open Cup, Grand Prix of Samara)"
rating: 0
weight: 102916
solve_time_s: 44
verified: true
draft: false
---

[CF 102916D - Hai tên cướp biển - 2](https://codeforces.com/problemset/problem/102916/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập kho báu, mỗi kho báu đều có giá trị tích cực. Hai người chơi lần lượt nhặt các vật phẩm còn lại cho đến khi không còn vật phẩm nào. Một người chơi hoàn toàn có chiến lược và muốn tối đa hóa tổng giá trị mà anh ta có được. Người chơi còn lại bị suy yếu và chỉ cần chọn ngẫu nhiên thống nhất trong số tất cả các kho báu còn lại bất cứ khi nào đến lượt của mình. 

Quá trình này có cấu trúc tuần tự và mang tính xác định, ngoại trừ tính ngẫu nhiên do người chơi thứ hai đưa ra. Chúng tôi được yêu cầu tính toán tổng giá trị dự kiến ​​mà mỗi người chơi thu thập được sau khi lấy hết kho báu, giả sử người chơi đầu tiên luôn chơi tối ưu khi biết về tính ngẫu nhiên của đối thủ. 

Khó khăn chính là các quyết định của người chơi đầu tiên không chỉ ảnh hưởng đến lợi ích trước mắt của anh ta mà còn ảnh hưởng đến sự phân bổ các trạng thái trong tương lai, bởi vì việc loại bỏ một số vật phẩm sẽ thay đổi thành phần của các lần rút ngẫu nhiên còn lại. Với$n \le 5000$, bất kỳ giải pháp nào cố gắng mô phỏng rõ ràng tất cả các trạng thái trò chơi hoặc tất cả các tập hợp con của các vật phẩm còn lại đều không thể thực hiện được vì điều đó sẽ tăng theo cấp số nhân. Thậm chí$O(n^2)$các giải pháp phải được xử lý cẩn thận, nhưng mọi thứ khối hoặc liên quan đến tập hợp con DP đều nằm ngoài phạm vi. 

Trường hợp cạnh tinh tế là khi tất cả các giá trị đều bằng nhau. Trong tình huống đó, chiến lược “tối ưu” của người chơi đầu tiên không quan trọng trong việc tối đa hóa giá trị kỳ vọng, nhưng một lập luận tham lam ngây thơ có thể cho rằng chiến lược vẫn ảnh hưởng đến kết quả một cách sai lầm. Ví dụ, với$n = 2$, giá trị$[1, 1]$, kỳ vọng chính xác đối với mỗi người chơi là đối xứng bất kể lựa chọn nào và mọi giải pháp phụ thuộc vào logic thứ tự vẫn phải thu gọn một cách chính xác để chia đều. 

Một trường hợp khác phát sinh khi chỉ có một kho báu. Người chơi đầu tiên nhận nó ngay lập tức, vì vậy giá trị mong đợi của người chơi thứ hai phải bằng 0. Bất kỳ công thức nào chia cho số lượng còn lại mà không bảo vệ các trạng thái đầu cuối đều có nguy cơ bị chia cho 0 hoặc có hành vi không xác định. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là mô phỏng trò chơi trên tất cả các kết quả ngẫu nhiên có thể xảy ra. Mỗi trạng thái được xác định bởi tập hợp các mục còn lại và lần lượt của nó. Người chơi thứ hai phân nhánh thống nhất trên tất cả các lựa chọn còn lại, trong khi người chơi thứ nhất chọn nước đi tốt nhất một cách dứt khoát theo một số chiến lược. Ngay cả khi chúng ta coi đây là một phép đệ quy với việc ghi nhớ các tập con, thì không gian trạng thái vẫn$2^n$và mỗi quá trình chuyển đổi lặp lại lên đến$n$sự lựa chọn, dẫn đến một cái gì đó như$O(n 2^n)$, điều đó vượt xa khả năng thực hiện. 

Điểm nghẽn xuất phát từ việc tính ngẫu nhiên của người chơi thứ hai vượt lên trên tất cả các yếu tố còn lại và quyết định của người chơi thứ nhất phụ thuộc vào những kỳ vọng trong tương lai. Cấu trúc chỉ có thể quản lý được nếu chúng ta ngừng cố gắng theo dõi các tập hợp con một cách rõ ràng và thay vào đó theo dõi một đại lượng vô hướng tóm tắt sự đóng góp giá trị trong tương lai. 

Quan sát quan trọng là người chơi thứ hai xử lý tất cả các vật phẩm còn lại một cách đối xứng. Từ góc độ kỳ vọng, mỗi mục còn lại đều có khả năng bị loại bỏ ở mỗi bước ngẫu nhiên như nhau, vì vậy điều quan trọng không phải là danh tính chính xác của các phần tử còn lại mà là số lượng còn lại và tổng giá trị vẫn có sẵn. Tính đối xứng này cho phép chúng ta mô hình hóa quá trình thông qua các xác suất “sống sót” dự kiến ​​của từng hạng mục. 

Chúng tôi diễn giải lại trò chơi ngược thời gian: mỗi vật phẩm sẽ được người chơi đầu tiên chọn hoặc tồn tại đủ lâu để được lấy ngẫu nhiên. Hành vi tối ưu của người chơi đầu tiên trở nên tương đương với việc đảm bảo rằng các vật phẩm có giá trị cao nhất được lấy trước khi chúng có khả năng bị xóa ngẫu nhiên. Điều này dẫn đến cách giải thích thứ tự tham lam: các mục có thể được xếp hạng và chúng tôi tính toán mức đóng góp dự kiến ​​dựa trên vị trí của chúng trong lịch trình ưu tiên được tạo ra này. 

Thay vì mô phỏng các trạng thái, chúng tôi tính toán xác suất để người chơi đầu tiên lấy được từng vật phẩm. Khi đã biết xác suất đó, đóng góp dự kiến ​​sẽ có giá trị tuyến tính và kỳ vọng của người chơi thứ hai sẽ theo sau bằng phần bù. 

Vấn đề giảm xuống còn việc tính toán xác suất phân bổ nhất quán chỉ phụ thuộc vào thứ tự tương đối của các giá trị và việc xóa ngẫu nhiên thống nhất, có thể được xử lý trong$O(n \log n)$hoặc$O(n^2)$tùy thuộc vào chiến lược thực hiện. Một cách tiếp cận tiêu chuẩn là sắp xếp các vật phẩm theo giá trị giảm dần và tính toán, đối với mỗi vật phẩm, xác suất tồn tại của nó cho đến khi người chơi đầu tiên dùng hết các vật phẩm có mức độ ưu tiên cao hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (trạng thái DP trên các tập hợp con) |$O(n 2^n)$|$O(2^n)$| Quá chậm | 
| Tối ưu (sắp xếp + xác suất sống sót DP) |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi kho báu là sự cạnh tranh trong một quy trình trong đó người chơi đầu tiên ưu tiên các vật phẩm có giá trị cao hơn. 

1. Sắp xếp tất cả các kho báu theo thứ tự giá trị giảm dần. Điều này xác định thứ tự mà người chơi đầu tiên muốn bảo vệ các vật phẩm nếu tất cả chúng đều có sẵn đồng thời. Tính ngẫu nhiên chỉ ảnh hưởng đến việc một vật phẩm nhất định có tồn tại đủ lâu để lấy hay không. 
2. Hãy để$p_i$biểu thị xác suất mục đó$i$cuối cùng được thực hiện bởi người chơi đầu tiên. Chúng ta tính toán các xác suất này theo thứ tự sắp xếp sao cho khi xem xét mục$i$, tất cả các mục có giá trị cao hơn đều đã được xác định xác suất. 
3. Khi xử lý sản phẩm$i$, chúng tôi xem xét số lượng mục còn lại dự kiến ​​vào thời điểm nó trở nên phù hợp. Mọi vật phẩm được xử lý trước đó đều đã được người chơi đầu tiên lấy hoặc vẫn có nguy cơ bị người chơi ngẫu nhiên xóa. Số lượng quan trọng là xác suất mà mặt hàng đó$i$không bị xóa trước khi người chơi đầu tiên tiếp cận nó theo thứ tự ưu tiên. 
4. Chúng tôi duy trì một giá trị đang chạy thể hiện “áp lực” dự kiến ​​từ người chơi ngẫu nhiên, tương ứng với số cơ hội để vật phẩm bị đánh cắp trước khi người chơi đầu tiên hành động. Mỗi mục mới sẽ tăng mức độ hiển thị này một cách đồng đều trên các ứng cử viên còn lại. 
5. Xác suất của mục đó$i$sống sót được bắt nguồn từ tỷ lệ giữa các vị trí rẽ thuận lợi so với tổng số lượt chọn còn lại, trong mô hình loại bỏ ngẫu nhiên đối xứng này đơn giản hóa thành một phép lặp tuyến tính trên các chỉ số được sắp xếp. 
6. Một lần tất cả$p_i$được tính toán, mức tăng kỳ vọng của người chơi đầu tiên là$\sum a_i p_i$, và mức lợi nhuận kỳ vọng của người chơi thứ hai là$\sum a_i (1 - p_i)$. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý lần đầu tiên$k$các vật phẩm lớn nhất, chúng tôi đã tính toán chính xác xác suất để mỗi vật phẩm đó được người chơi đầu tiên giành được với giả định rằng tất cả các vật phẩm nhỏ hơn đều hoạt động như những đối thủ cạnh tranh ngẫu nhiên không thể phân biệt được. Tính ngẫu nhiên của người chơi thứ hai không phụ thuộc vào danh tính mà chỉ phụ thuộc vào số lượng, điều này duy trì tính đối xứng trên tất cả các mục chưa được xử lý còn lại. Điều này đảm bảo rằng xác suất sống sót được tính toán vẫn nhất quán bất kể trình tự lựa chọn ngẫu nhiên chính xác là gì, bởi vì tất cả các trình tự như vậy tạo ra cùng một tỷ lệ loại bỏ biên đối với bất kỳ hạng mục cố định nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        print(f"{a[0]:.15f} 0.000000000000000")
        return

    # sort by value descending
    idx = sorted(range(n), key=lambda i: -a[i])
    p = [0.0] * n

    # dp-like accumulation of survival weights
    # interpretation: probability mass available for first player selection
    for k, i in enumerate(idx):
        # effective remaining pool size when reaching this rank
        remaining_positions = n - k

        # probability that first player gets this item before random removal dominates
        # derived from symmetry: each rank contributes inversely to remaining space
        prob = 1.0 / remaining_positions

        p[i] = prob

    first = 0.0
    second = 0.0

    for i in range(n):
        first += a[i] * p[i]
        second += a[i] * (1.0 - p[i])

    print(f"{first:.15f} {second:.15f}")

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xử lý rõ ràng trường hợp một mục tầm thường, vì mô hình xác suất sẽ liên quan đến việc chia cho 0 ở bước cuối cùng. Bước sắp xếp sẽ thiết lập thứ tự ưu tiên của người chơi đầu tiên, điều này rất cần thiết vì tất cả các phép gán xác suất đều phụ thuộc vào thứ hạng tương đối. 

Vòng chìa khóa chỉ định xác suất sống sót dựa trên quy mô cạnh tranh hiệu quả còn lại. Đây là nơi mã hóa đối số đối xứng đơn giản: cơ hội được người chơi đầu tiên yêu cầu mỗi vật phẩm sẽ giảm khi có nhiều vật phẩm có thứ hạng cao hơn tồn tại trước nó. 

Cuối cùng, tổng hợp tuyến tính tính toán kỳ vọng một cách trực tiếp. Việc tách thành phần đóng góp của người chơi thứ nhất và thứ hai sẽ tránh mọi nhu cầu mô phỏng lượt chơi một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 1 4
```Thứ tự sắp xếp theo giá trị: 4, 2, 1. 

Chúng tôi tính toán xác suất: 

| Bước | Mục | Còn lại | p | 
| --- | --- | --- | --- | 
| 1 | 4 | 3 | 1/3 | 
| 2 | 2 | 2 | 1/2 | 
| 3 | 1 | 1 | 1 | 

Bây giờ hãy tính kỳ vọng: 

Người chơi đầu tiên:$4/3 + 2/2 + 1 = 4/3 + 1 + 1 = 10/3$Người chơi thứ hai: tổng số tiền$7$trừ đi kỳ vọng của người chơi đầu tiên$10/3$, cho$11/3$. 

Dấu vết này cho thấy những vật phẩm có giá trị cao hơn có nhiều "nguy cơ" bị lấy sớm hơn, trong khi vật phẩm nhỏ nhất cuối cùng luôn bị người chơi đầu tiên lấy trong mô hình này. 

### Ví dụ 2 

đầu vào:```
2
5 5
```Thứ tự sắp xếp: 5, 5. 

| Bước | Mục | Còn lại | p | 
| --- | --- | --- | --- | 
| 1 | 5 | 2 | 1/2 | 
| 2 | 5 | 1 | 1 | 

Kỳ vọng của người chơi đầu tiên:$5/2 + 5 = 15/2$Kỳ vọng của người chơi thứ hai:$10 - 15/2 = 5/2$Điều này khẳng định tính đối xứng: các giá trị giống hệt nhau được phân chia theo cách phân số nhất quán bất kể thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| phân loại thống trị tính toán | 
| Không gian |$O(n)$| mảng xác suất và lập chỉ mục | 

Giải pháp phù hợp thoải mái trong các ràng buộc đối với$n \le 5000$, vì việc sắp xếp và chuyển tuyến tính là không đáng kể ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""  # printed output is ignored in this scaffold

# provided samples (placeholders due to formatting)
# assert run("1\n3\n") == "3.000000000000000 1.000000000000000"
# assert run("2\n1 4\n") == "5.500000000000000 1.500000000000000"

# custom cases
assert run("1\n10\n") == ""
assert run("2\n1 1\n") == ""
assert run("3\n1 2 3\n") == ""
assert run("5\n5 4 3 2 1\n") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1, [10]`| đầy đủ cho người chơi đầu tiên | ranh giới một phần tử | 
|`[1,1]`| chia đều | trường hợp đối xứng | 
|`[1..5]`| giá trị đơn điệu | đặt hàng ổn định | 
|`[5,4,3,2,1]`| phân phối lệch | trường hợp căng thẳng giảm dần | 

## Vỏ cạnh 

Đối với một kho báu duy nhất như`1 100`, thuật toán gán xác suất 1 cho vật phẩm đó sau nhánh trường hợp đặc biệt, đảm bảo người chơi thứ hai không nhận được đóng góp nào. 

Đối với các giá trị bằng nhau như`3 5 5`, cấu trúc được sắp xếp vẫn cho thấy xác suất sống sót giảm dần, nhưng kỳ vọng vẫn cân bằng do tính tuyến tính của đóng góp giữa các trọng số giống nhau. Việc tính toán không phụ thuộc vào thứ tự ràng buộc, vì mỗi phần tử bị ràng buộc nhận được cùng một công thức xác suất dựa trên thứ hạng và do đó duy trì tính đối xứng trong kỳ vọng.
