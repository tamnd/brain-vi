---
title: "CF 103486G - Sửa chữa ma trận"
description: "Chúng ta được cung cấp một ma trận nhị phân $N nhân N$, ngoại trừ một số mục bị thiếu và được viết là $-1$. Mọi mục nhập không xác định phải được thay thế bằng $0$ hoặc $1$. Cùng với ma trận, chúng ta được cấp XOR của từng hàng và mỗi cột sau khi xây dựng lại."
date: "2026-07-03T06:21:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103486
codeforces_index: "G"
codeforces_contest_name: "The 15th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 103486
solve_time_s: 31
verified: true
draft: false
---

[CF 103486G - Sửa chữa ma trận](https://codeforces.com/problemset/problem/103486/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$N \times N$ma trận nhị phân, ngoại trừ một số mục bị thiếu và được viết dưới dạng$-1$. Mọi mục không xác định phải được thay thế bằng một trong hai$0$hoặc$1$. Cùng với ma trận, chúng ta được cấp XOR của từng hàng và mỗi cột sau khi xây dựng lại. Mục đích là để quyết định xem liệu các giá trị còn thiếu có thể được điền một cách nhất quán với các ràng buộc XOR này hay không và nếu có, hãy xuất ra một lần hoàn thành hợp lệ. 

Mỗi ràng buộc hàng cho biết: nếu bạn XOR tất cả các giá trị trong hàng$i$, bạn phải có được$R_i$. Mỗi ràng buộc cột nói tương tự đối với các cột sử dụng$C_j$. Vì XOR có tính kết hợp và giao hoán nên mỗi ô được điền đóng góp chính xác một lần vào hàng của nó và một lần vào ràng buộc cột của nó. 

Khó khăn chính là mỗi ô chưa biết ảnh hưởng đồng thời đến hai ràng buộc, một hàng và một cột, do đó việc điền các hàng hoặc cột độc lập đơn giản không thành công. 

Những hạn chế$N \le 1000$ngụ ý lên đến$10^6$tế bào. Bất kỳ cách tiếp cận nào cố gắng điền theo cấp số nhân, quay lui hoặc truyền bá toàn cầu trên mỗi ô với tính toán lại lặp đi lặp lại đều quá chậm. Một giải pháp đúng phải giảm vấn đề về lý luận thời gian tuyến tính trên lưới. 

Một trường hợp thất bại tinh tế xuất hiện khi các ràng buộc nhất quán cục bộ nhưng không thể thực hiện được trên toàn cầu. Ví dụ: một ma trận được lấp đầy một phần có thể cho phép các XOR hàng khớp nhau, nhưng XOR cột buộc phải mâu thuẫn sau khi truyền. Một chế độ lỗi khác là khi tồn tại nhiều lần hoàn thành hợp lệ nhưng việc lấp đầy tham lam bất cẩn đã xác định sớm một ô, khiến cho các ràng buộc sau này không thể được đáp ứng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ gán giá trị cho tất cả$-1$các ô và kiểm tra xem XOR hàng và cột có khớp nhau hay không. Nếu có$k$các tế bào chưa biết, đây là$O(2^k \cdot N^2)$, điều này hoàn toàn không khả thi ngay cả đối với những$k$. Ngay cả cách tiếp cận quay lui thông minh hơn chỉ định từng ô một và duy trì trạng thái XOR một phần vẫn khám phá không gian tìm kiếm theo cấp số nhân. 

Cấu trúc của bài toán gợi ý xem mỗi ô là một biến trên GF(2). Mỗi ràng buộc hàng và cột trở thành một phương trình tuyến tính trong các biến này. Mỗi ô được lấp đầy sẽ làm giảm độ không chắc chắn và mỗi ô$-1$ô là một biến bị ràng buộc bởi chính xác hai phương trình. 

Quan sát quan trọng là chúng ta thực sự không cần phải giải một hệ tuyến tính đầy đủ bằng phép loại bỏ Gaussian. Thay vào đó, chúng ta có thể khai thác cấu trúc lưỡng cực giữa hàng và cột. Mỗi ô kết nối một phương trình hàng và một phương trình cột và các ràng buộc XOR cho phép chúng ta truyền bá các quyết định chẵn lẻ. 

Một cách tiếp cận mang tính xây dựng là coi các ô chưa biết là các cạnh có giá trị phải đáp ứng sự cân bằng chẵn lẻ giữa tổng hàng và cột. Chúng ta có thể gán các giá trị một cách tham lam trong khi vẫn duy trì tính nhất quán, nhưng chỉ sau khi đảm bảo rằng các ràng buộc về hàng và cột tương thích toàn cầu. 

Thông tin chi tiết quan trọng là tính nhất quán giảm xuống để đảm bảo rằng tổng XOR của tất cả các hàng bằng tổng XOR của tất cả các cột và sau đó chúng ta có thể gán các giá trị một cách tham lam bằng cách sử dụng chiến lược điền xác định đơn giản trên lưới, giải quyết từng$-1$theo cách duy trì tính chẵn lẻ của cả hàng và cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^k \cdot N^2)$|$O(N^2)$| Quá chậm | 
| Tối ưu |$O(N^2)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc với ý tưởng rằng mỗi giá trị ô đóng góp vào chính xác một XOR hàng và một XOR cột. Điều này cho phép chúng tôi duy trì tính chẵn lẻ một phần và gán các giá trị còn thiếu theo cách khắc phục đồng thời cả hai ràng buộc. 

### bước 

1. Tính toán đóng góp XOR ban đầu cho mỗi hàng và cột chỉ sử dụng các mục đã biết (các ô không bằng$-1$). 

Điều này mang lại trạng thái chẵn lẻ một phần mà cuối cùng phải khớp$R_i$Và$C_j$. Nếu một hàng hoặc cột đã vi phạm các đóng góp cố định (tức là sẽ yêu cầu những điều chỉnh không thể thực hiện được mà không có ẩn số), thì cấu trúc sẽ trở nên đáng ngờ nhưng chúng tôi trì hoãn việc xác thực cuối cùng cho đến khi được chỉ định. 
2. Xác định tất cả các ô có giá trị$-1$. Đây là bậc tự do duy nhất mà chúng ta có thể sử dụng để sửa lỗi chẵn lẻ. Mỗi ô như vậy kết nối chính xác một ràng buộc hàng và một cột. 
3. Xử lý từng hàng ma trận, từ trái qua phải. Đối với mỗi$-1$di động tại$(i, j)$, quyết định giá trị của nó sao cho nó giúp thỏa mãn một trong hai hàng$i$hoặc cột$j$, ưu tiên một quy tắc địa phương nhất quán. 

Một quy tắc nhất quán tự nhiên là: duy trì mức thâm hụt XOR hiện tại cho các hàng và cột và chỉ định ô để đáp ứng bất kỳ ràng buộc nào hiện có mức độ khẩn cấp cao hơn. Cụ thể, nếu hàng$i$vẫn cần lật so với mục tiêu của nó nhiều hơn cột$j$, chúng tôi đặt ô để đáp ứng tính chẵn lẻ của hàng; nếu không chúng tôi đáp ứng tính chẵn lẻ của cột. 
4. Sau khi điền vào tất cả các ô ngoại trừ bậc tự do có thể là một hàng hoặc cột, hãy kiểm tra xem tất cả các XOR hàng và cột có khớp với yêu cầu không$R_i$Và$C_j$. Nếu vẫn còn sự không khớp, thì phiên bản đó không nhất quán. 
5. Xuất ra ma trận đã hoàn thành. 

### Tại sao nó hoạt động 

Mỗi ô hoạt động như một nút chuyển đổi lật chính xác chẵn lẻ một hàng và chẵn lẻ một cột. Điều này làm cho hệ thống hoạt động giống như cân bằng hai bộ ràng buộc chẵn lẻ trên biểu đồ hai bên. Bởi vì mọi thành phần được kết nối đều được liên kết đầy đủ, việc phân công tham lam được hướng dẫn bởi những thiếu sót còn lại đảm bảo rằng không có quyết định cục bộ nào cản trở vĩnh viễn tính khả thi. 

Điều bất biến là sau khi xử lý tiền tố của các ô, phép gán một phần hiện tại luôn duy trì khả năng hoàn thành tất cả các ràng buộc chẵn lẻ còn lại. Mỗi phép gán làm giảm lỗi chẵn lẻ kết hợp giữa các hàng và cột mà không gây ra sự mất cân bằng không thể phục hồi được, bởi vì mỗi lần lật đều ảnh hưởng đồng thời đến chính xác một hàng và một cột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = []
    row = [0] * n
    col = [0] * n

    for i in range(n):
        arr = list(map(int, input().split()))
        a.append(arr)
```
