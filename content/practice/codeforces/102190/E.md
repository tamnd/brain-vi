---
title: "CF 102190E - đầu vào/đầu ra tiêu chuẩn"
description: "Vấn đề này khác với nhiệm vụ phân loại thuật toán thông thường. Bạn được cung cấp các tham số đã được đào tạo của một mạng lưới thần kinh tích chập nhỏ, theo sau là một bộ sưu tập gồm 28 x 28 hình ảnh nhị phân."
date: "2026-08-19T05:51:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102190
codeforces_index: "E"
codeforces_contest_name: "2019 ECNU Campus Invitational Contest"
rating: 0
weight: 102190
solve_time_s: 624
verified: true
draft: false
---

[CF 102190E - đầu vào/đầu ra tiêu chuẩn](https://codeforces.com/problemset/problem/102190/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề này khác với nhiệm vụ phân loại thuật toán thông thường. Bạn được cung cấp các tham số đã được đào tạo của một mạng lưới thần kinh tích chập nhỏ, theo sau là một bộ sưu tập gồm 28 x 28 hình ảnh nhị phân. Công việc của bạn là tái tạo lại đường chuyển tiếp của mạng và in chữ cái dự đoán cho mỗi hình ảnh. 

Có bốn lớp có thể. Đầu ra lớp 0 như`C`, lớp 1 như`E`, lớp 2 như`N`, và lớp 3 là`U`. Đầu vào bắt đầu bằng một số ma thuật không liên quan, sau đó đưa ra tất cả các trọng số và độ lệch của mạng theo một thứ tự cố định. Sau đó là số lượng hình ảnh và số pixel của mỗi hình ảnh. 

Bản thân mạng có cấu trúc cố định. Một hình ảnh 28 x 28 trước tiên đi qua bốn bộ lọc tích chập 5 x 5, tạo ra bốn bản đồ đặc trưng 24 x 24. Mỗi bản đồ được giảm tổng hợp tối đa 2 x 2 xuống còn 12 x 12. Sau đó, ReLU được áp dụng. Kết quả trải qua chín bộ lọc tích chập 3 x 3, trong đó mỗi bộ lọc đầu ra kết hợp tất cả bốn kênh đầu vào, tạo ra chín bản đồ 10 x 10. Một hoạt động tổng hợp tối đa 2 x 2 khác làm giảm các bản đồ này xuống còn chín bản đồ 5 x 5, tiếp theo là ReLU. Các giá trị 9 nhân 5 nhân 5 này được làm phẳng thành 225 số, được chuyển qua lớp được kết nối đầy đủ từ 225 đến 64 và ReLU, và cuối cùng qua lớp được kết nối đầy đủ từ 64 đến 4. Điểm lớn nhất trong bốn điểm cuối cùng quyết định lớp. 

Các trọng số tích chập được lưu trữ ở dạng hai chiều phẳng. Đối với phép chập đầu tiên, tensor 4 x 1 x 5 x 5 trở thành 4 hàng có 25 giá trị. Đối với phép tích chập thứ hai, tensor 9 x 4 x 3 x 3 trở thành 9 hàng gồm 36 giá trị. Thứ tự hàng tuân theo thứ tự kênh, hàng, cột tự nhiên, do đó, một mục nhập hạt nhân phẳng có thể được lập chỉ mục trực tiếp bằng số học thích hợp. 

Kích thước hình ảnh cố định giúp cho việc tính toán có thể quản lý được ngay cả với các vòng lặp lồng nhau đơn giản. Chỉ có 2240 hình ảnh trong dữ liệu thử nghiệm dự định và mỗi hình ảnh chứa 784 pixel. Phép toán lặp lại lớn nhất là phép tích chập, nhưng kích thước của nó nhỏ: phép tích chập đầu tiên thực hiện 4 lần 24 lần 24 lần 25 phép cộng nhân và phép tích thứ hai thực hiện 9 lần 10 lần 10 lần 36. Các lớp được kết nối đầy đủ cũng nhỏ, với 225 nhân 64 và 64 lần 4 phép cộng cho mỗi hình ảnh. Đây là vài triệu phép tính cơ bản trên một tập hợp đầu vào hoàn chỉnh, không phải là một phép tính lớn tiệm cận. 

Mối nguy hiểm chính không phải là độ phức tạp tiệm cận mà là việc tái tạo một cách trung thực bố cục tensor và thứ tự hoạt động của mạng. Tích chập phải sử dụng các cửa sổ hợp lệ với bước một, gộp tối đa phải sử dụng các cửa sổ 2 x 2 không chồng chéo và tích chập thứ hai phải tính tổng trên tất cả bốn kênh đầu vào. Áp dụng ReLU ở giai đoạn sai sẽ làm thay đổi mạng. 

Trường hợp cạnh đơn giản là một hình ảnh chỉ chứa 0 pixel. Giả sử mọi trọng số đều bằng 0 và mọi độ lệch đều bằng 0. Mọi giá trị trung gian đều bằng 0 và cả bốn điểm cuối cùng đều bằng 0. Dự đoán đúng là`C`, bởi vì lớp 0 thắng argmax thông thường khi hòa. Ví dụ: một triển khai khởi tạo lớp tốt nhất thành 1 sẽ in không chính xác`E`. 

Một trường hợp tinh vi khác xảy ra khi tích chập có kết quả âm tính. Ví dụ: nếu một đầu ra của tích chập đầu tiên là`-3`, việc gộp tối đa được thực hiện trước ReLU đầu tiên, do đó giá trị đó có thể cạnh tranh với các giá trị khác trong cửa sổ gộp 2 x 2 của nó. Nếu bốn giá trị là`-3, -5, -7, -2`, tổng hợp tối đa tạo ra`-2`, và chỉ khi đó ReLU mới thay đổi nó thành 0. Việc áp dụng ReLU trước khi gộp sẽ mang lại mức tối đa tương tự trong ví dụ cụ thể này, nhưng đây không phải là lý do để di chuyển hoạt động một cách tùy tiện qua mạng. Thứ tự đã chỉ định phải được triển khai trực tiếp, đặc biệt vì phép tích chập thứ hai cũng được theo sau bởi phép gộp và sau đó là ReLU. 

Ranh giới của mọi tích chập là một nguồn lỗi phổ biến khác. Cửa sổ 5 x 5 trên hình ảnh 28 x 28 có 24 vị trí bắt đầu hợp lệ theo mỗi hướng, cụ thể là các hàng từ 0 đến 23 và các cột từ 0 đến 23. Bắt đầu từ hàng 24 sẽ yêu cầu truy cập vào hàng 28, nằm ngoài hình ảnh. Tương tự, cửa sổ 3 x 3 trên bản đồ đặc trưng 12 x 12 có các vị trí bắt đầu từ 0 đến 9, cho kết quả 10 x 10. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là triển khai mạng lưới thần kinh chính xác như mô tả. Đối với mọi vị trí đầu ra của tích chập, hãy lặp qua các hàng và cột hạt nhân, đồng thời đối với tích chập thứ hai, trên mọi kênh đầu vào. Thêm độ lệch sau tổng trọng số tương ứng. Sau đó, thực hiện gộp tối đa với các cửa sổ 2 x 2 rõ ràng, áp dụng ReLU, làm phẳng tensor kết quả theo thứ tự từ điển chính của kênh và đánh giá hai lớp dày đặc. 

Việc triển khai bạo lực này đã đủ nhanh vì kích thước mạng cố định và nhỏ. Đối với một hình ảnh, tích chập đầu tiên có giá 4 lần 24 lần 24 lần 25 = 57.600 phép cộng. Cái thứ hai có giá 9 lần 10 lần 10 lần 4 lần 9 = 32.400 phép cộng. Lớp dày đặc đầu tiên có giá 14.400 phép nhân và lớp thứ hai có giá 256. Tổng cộng là khoảng 105.000 phép nhân cho mỗi hình ảnh, hoặc khoảng 235 triệu phép nhân cho 2240 hình ảnh. 

Số lượng đó có vẻ lớn trong Python, vì vậy việc tối ưu hóa hữu ích không phải là một thuật toán toán học mới. Quan sát chính là kiến ​​trúc cố định, đầu vào là nhị phân và các bộ lọc đã được huấn luyện tương tự được sử dụng lại cho mọi hình ảnh. Chúng ta có thể duy trì việc triển khai dưới dạng các vòng lặp lồng nhau đơn giản trong khi giảm chi phí cấp Python bằng cách lưu trữ các tensor dưới dạng mảng phẳng và sử dụng các biến cục bộ. Vì vấn đề không có kích thước hình ảnh thay đổi và chỉ có khoảng hai nghìn hình ảnh, nên một đường chuyển tiếp trực tiếp được viết cẩn thận là giải pháp thích hợp. 

Một cách tiếp cận phức tạp hơn sẽ cố gắng suy ra các chữ cái trực tiếp từ hình ảnh hoặc huấn luyện một bộ phân loại riêng biệt. Điều đó là không cần thiết và có khả năng kém tin cậy hơn nhiều. Các tham số được cung cấp đã mã hóa bộ phân loại với độ chính xác được đảm bảo trên ngưỡng chấp nhận, do đó, việc tái tạo các tham số đó là giải pháp xác định dự kiến. 

Chi tiết triển khai quan trọng là cách bố trí tensor. Các trọng số tích chập đã được nhóm theo kênh đầu ra, sau đó là kênh đầu vào, sau đó là hàng và cột hạt nhân. Tương tự như vậy, đầu ra tích chập thứ hai được làm phẳng phải được sắp xếp theo giá trị 5 x 5 của kênh 0, theo sau là giá trị 5 x 5 của kênh 1, v.v. Điều đó khớp chính xác với thứ tự từ điển đã nêu của các chỉ số tensor. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuyển tiếp mạng thần kinh trực tiếp | O(t) với hằng số cố định khoảng 105.000 phép tính số học trên mỗi hình ảnh | O(1) trên mỗi hình ảnh ngoài các tham số mạng cố định | Đã chấp nhận | 
| Xây dựng lại hoặc đào tạo một bộ phân loại khác | Phụ thuộc vào phương pháp, có khả năng lớn hơn nhiều | Phụ thuộc vào mô hình | Không cần thiết | 

## Hướng dẫn thuật toán

1. Đọc và loại bỏ con số kỳ diệu. Sau đó đọc bốn nhóm tham số mạng theo thứ tự được chỉ định của chúng: trọng số và độ lệch của tích chập đầu tiên, trọng số và độ lệch của tích chập thứ hai, trọng số và độ lệch của lớp dày đặc đầu tiên cũng như trọng số và độ lệch của lớp dày đặc cuối cùng. Kích thước của chúng là cố định, vì vậy mỗi kích thước có thể được lưu trữ dưới dạng danh sách Python phẳng. 
2. Đọc số lượng hình ảnh và xử lý từng hình ảnh 28 x 28 một cách độc lập. Chỉ giữ lại hình ảnh hiện tại sẽ tránh lưu trữ tất cả 2240 hình ảnh cùng một lúc. 
3. Tính tích chập đầu tiên. Đối với mọi kênh đầu ra và mọi cửa sổ 5 x 5 hợp lệ, hãy tính tổng trọng số của 25 pixel đầu vào tương ứng và thêm độ lệch của kênh. Vì đầu vào có một kênh nên không có vòng lặp kênh ở đây. Đầu ra có hình dạng 4 x 24 x 24. 
4. Áp dụng tính năng gộp tối đa 2 x 2 một cách độc lập cho từng kênh trong số bốn kênh. Các cửa sổ gộp không chồng lên nhau, do đó vị trí đầu ra`(r, c)`đọc hàng`2*r`Và`2*r+1`và cột`2*c`Và`2*c+1`. Kết quả có hình dạng 4 x 12 x 12. 
5. Áp dụng ReLU cho mọi giá trị gộp. ReLU thay thế mọi giá trị âm bằng 0 và giữ nguyên các giá trị không âm. 
6. Tính tích chập thứ hai. Đối với mỗi kênh trong số chín kênh đầu ra, mỗi vị trí đầu ra 10 x 10 sử dụng cửa sổ 3 x 3 từ mỗi một trong bốn kênh đầu vào. 36 giá trị hạt nhân tương ứng với một kênh đầu ra được nhân với bốn cửa sổ 3 x 3 đó và tính tổng, theo sau là độ lệch đầu ra. 
7. Áp dụng thao tác gộp tối đa 2 x 2 khác, giảm mỗi kênh 10 x 10 xuống còn 5 x 5. Sau đó áp dụng ReLU cho tất cả 225 giá trị kết quả. 
8. Làm phẳng tensor 9 x 5 x 5 theo thứ tự kênh chính. Tất cả 25 giá trị của kênh 0 đều xuất hiện trước, sau đó là tất cả 25 giá trị của kênh 1, v.v. Điều này tạo ra chính xác 225 đầu vào cho lớp được kết nối đầy đủ đầu tiên. 
9. Đánh giá lớp dày đặc đầu tiên. Đối với mỗi trong số 64 nơ-ron của nó, hãy tính tích số chấm giữa 225 trọng số của nó và vectơ đặc trưng đã được làm phẳng, sau đó cộng độ lệch của nó. Áp dụng ReLU cho 64 giá trị thu được. 
10. Đánh giá lớp dày đặc cuối cùng. Mỗi điểm trong số bốn điểm đầu ra là tích số chấm của vectơ ẩn 64 chiều với một hàng của ma trận trọng số cuối cùng, cộng với độ lệch tương ứng. Dự đoán là chỉ số của điểm số lớn nhất. 
11. Chuyển đổi chỉ số chiến thắng thành chữ cái của nó bằng cách sử dụng ánh xạ`0 -> C`,`1 -> E`,`2 -> N`, Và`3 -> U`, sau đó in nó. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi giai đoạn, chương trình sẽ lưu trữ chính xác tensor mà mạng nơron được chỉ định sẽ tạo ra cho hình ảnh hiện tại. Tích chập đầu tiên tính toán mọi tích số chấm cửa sổ nhân hợp lệ với độ lệch chính xác, do đó bốn bản đồ đặc trưng của nó là chính xác. Sau đó, tổng hợp tối đa sẽ chọn mức tối đa từ mọi cửa sổ 2 x 2 được chỉ định và ReLU thực hiện chuyển đổi theo phần tử cần thiết. Lập luận tương tự áp dụng cho giai đoạn tích chập và gộp thứ hai. Bởi vì vectơ phẳng tuân theo thứ tự kênh, hàng, cột từ điển cần thiết, cả hai lớp được kết nối đầy đủ đều nhận được chính xác các giá trị mà mạng được đào tạo mong đợi. Do đó, argmax cuối cùng chọn cùng lớp với mô hình ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    magic = input()

    conv1_w = [list(map(float, input().split())) for _ in range(4)]
    conv1_b = list(map(float, input().split()))

    conv2_w = [list(map(float, input().split())) for _ in range(9)]
    conv2_b = list(map(float, input().split()))

    fc1_w = [list(map(float, input().split())) for _ in range(64)]
    fc1_b = list(map(float, input().split()))

    fc2_w = [list(map(float, input().split())) for _ in range(4)]
    fc2_b = list(map(float, input().split()))

    t = int(input())
    letters = "CENU"
    output = []

    for _ in range(t):
        image = []
        for _ in range(28):
            image.extend(map(float, input().split()))

        # Conv 1: 1 -> 4, 28x28 -> 24x24.
        conv1 = [0.0] * (4 * 24 * 24)

        for oc in range(4):
            kernel = conv1_w[oc]
            bias = conv1_b[oc]
            base_out = oc * 24 * 24

            for r in range(24):
                out_base = base_out + r * 24
                img_base = r * 28

                for c in range(24):
                    s = bias
                    k = 0
                    for kr in range(5):
                        row_base = img_base + kr * 28 + c
                        for kc in range(5):
                            s += kernel[k] * image[row_base + kc]
                            k += 1

                    conv1[out_base + c] = s

        # Max-pooling: 24x24 -> 12x12.
        pool1 = [0.0] * (4 * 12 * 12)

        for ch in range(4):
            src_base = ch * 24 * 24
            dst_base = ch * 12 * 12

            for r in range(12):
                sr = 2 * r
                for c in range(12):
                    sc = 2 * c

                    a = conv1[src_base + sr * 24 + sc]
                    b = conv1[src_base + sr * 24 + sc + 1]
                    d = conv1[src_base + (sr + 1) * 24 + sc]
                    e = conv1[src_base + (sr + 1) * 24 + sc + 1]

                    pool1[dst_base + r * 12 + c] = max(a, b, d, e)

        # ReLU.
        for i in range(len(pool1)):
            if pool1[i] < 0:
                pool1[i] = 0.0

        # Conv 2: 4 -> 9, 12x12 -> 10x10.
        conv2 = [0.0] * (9 * 10 * 10)

        for oc in range(9):
            kernel = conv2_w[oc]
            bias = conv2_b[oc]
            dst_base = oc * 100

            for r in range(10):
                for c in range(10):
                    s = bias

                    for ic in range(4):
                        src_base = ic * 144
                        kernel_base = ic * 9

                        for kr in range(3):
                            src_row = src_base + (r + kr) * 12 + c
                            krow = kernel_base + kr * 3

                            s += (
                                kernel[krow] * pool1[src_row]
                                + kernel[krow + 1] * pool1[src_row + 1]
                                + kernel[krow + 2] * pool1[src_row + 2]
                            )

                    conv2[dst_base + r * 10 + c] = s

        # Max-pooling: 10x10 -> 5x5, followed by ReLU.
        features = [0.0] * (9 * 25)

        for ch in range(9):
            src_base = ch * 100
            dst_base = ch * 25

            for r in range(5):
                sr = 2 * r
                for c in range(5):
                    sc = 2 * c

                    a = conv2[src_base + sr * 10 + sc]
                    b = conv2[src_base + sr * 10 + sc + 1]
                    d = conv2[src_base + (sr + 1) * 10 + sc]
                    e = conv2[src_base + (sr + 1) * 10 + sc + 1]

                    value = max(a, b, d, e)
                    if value < 0:
                        value = 0.0

                    features[dst_base + r * 5 + c] = value

        # FC 1: 225 -> 64, followed by ReLU.
        hidden = [0.0] * 64

        for i in range(64):
            row = fc1_w[i]
            s = fc1_b[i]

            for j in range(225):
                s += row[j] * features[j]

            if s < 0:
                s = 0.0

            hidden[i] = s

        # FC 2: 64 -> 4.
        scores = [0.0] * 4

        for i in range(4):
            row = fc2_w[i]
            s = fc2_b[i]

            for j in range(64):
                s += row[j] * hidden[j]

            scores[i] = s

        best = 0
        for i in range(1, 4):
            if scores[i] > scores[best]:
                best = i

        output.append(letters[best])

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp đầu vào trước tiên tiêu thụ chính xác số lượng hàng tham số cố định. Một tensor trọng số tích chập được biểu diễn dưới dạng một hàng phẳng cho mỗi kênh đầu ra, do đó`conv1_w[oc]`chứa 25 giá trị cho một bộ lọc 5 x 5, trong khi`conv2_w[oc]`chứa bốn hạt nhân 3 x 3 liên tiếp. 

Tích chập đầu tiên sử dụng kích thước đầu ra 24 x 24 vì hạt nhân 5 x 5 phải nằm hoàn toàn bên trong hình ảnh 28 x 28. Giới hạn vòng lặp`range(24)`do đó không phải là hằng số tùy ý, chúng là số vị trí cửa sổ hợp lệ chính xác. 

Giai đoạn gộp đầu tiên truy cập rõ ràng bốn giá trị lân cận. Điều này tránh tạo ra các danh sách hai chiều tạm thời và cũng làm cho bước tiến không chồng chéo của hai danh sách trở nên rõ ràng. ReLU được áp dụng sau khi gộp, khớp với định nghĩa mạng. 

Tích chập thứ hai là phần có nhiều khả năng được triển khai không chính xác nhất. Một kênh đầu ra có 4 kênh đầu vào và mỗi kênh đầu vào đóng góp một sản phẩm hoàn chỉnh có kích thước 3 x 3 chấm. Phần bù hạt nhân`ic * 9`chọn khối 3 x 3 thích hợp từ hàng phẳng. 

Sau giai đoạn gộp thứ hai, mỗi kênh chứa chính xác 25 giá trị. Vì các kênh được lưu trữ liên tiếp nên kết quả`features`danh sách đã ở thứ tự làm phẳng cần thiết. Không cần thực hiện thao tác chuyển vị hoặc định hình lại. 

Các lớp dày đặc sử dụng trực tiếp các ma trận được cung cấp. Số nguyên Python không phải là vấn đề ở đây vì trọng số là giá trị dấu phẩy động và tổng vẫn là giá trị dấu phẩy động. argmax cuối cùng cố tình sử dụng`>`còn hơn là`>=`, do đó, các mối quan hệ vẫn được gán cho chỉ mục lớp thấp nhất, phù hợp với quy ước argmax thông thường. 

## Ví dụ đã hoạt động 

Các ma trận mẫu thực tế của câu lệnh bị bỏ qua trong văn bản bài toán được cung cấp, do đó, quá trình chuyển tiếp số hoàn chỉnh của chúng không thể được xây dựng lại ở đây. Hai dấu vết tổng hợp nhỏ sau đây sử dụng các tham số mạng được đơn giản hóa để thể hiện cùng một luồng điều khiển. 

### Ví dụ 1 

Hãy xem xét một đầu vào khái niệm có mạng tạo ra bốn điểm cuối cùng sau: 

| Sân khấu | Giá trị | 
| --- | --- | 
| FC2 điểm 0 | 2,5 | 
| FC2 điểm 1 | -1.0 | 
| FC2 điểm 2 | 0,7 | 
| FC2 điểm 3 | 1.8 | 
| Argmax | 0 | 
| Đầu ra |`C`| 

Điểm chiến thắng là điểm đầu tiên nên bộ phân loại sẽ in`C`. Dấu vết chứng tỏ rằng chương trình không áp dụng softmax trước khi chọn lớp. Softmax bảo toàn thứ tự của điểm số, do đó việc tính toán nó sẽ thêm công việc mà không làm thay đổi dự đoán. 

### Ví dụ 2 

Hãy xem xét điểm số cuối cùng khi một số lớp ngang nhau: 

| Sân khấu | Giá trị | 
| --- | --- | 
| FC2 điểm 0 | 0,0 | 
| FC2 điểm 1 | 0,0 | 
| FC2 điểm 2 | -2.0 | 
| FC2 điểm 3 | 0,0 | 
| Argmax sau khi quét | 0 | 
| Đầu ra |`C`| 

Chỉ số tốt nhất ban đầu là 0 và chương trình chỉ thay thế nó khi gặp điểm lớn hơn. Do đó, điểm bằng nhau khiến lớp 0 được chọn. Đây là hành vi xác định chính xác cho một argmax từ trái sang phải thông thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) với hằng số cố định | Mọi hình ảnh đều có kích thước 28 x 28 giống nhau và đi qua mạng có kích thước cố định | 
| Không gian | O(1) mỗi hình ảnh | Tất cả các kích thước mạng đều cố định và chỉ các tensor trung gian của một hình ảnh được lưu trữ | 

Đối với 2240 hình ảnh dự định, việc triển khai trực tiếp thực hiện khoảng 235 triệu phép cộng nhân dấu phẩy động, cùng với công việc gộp và lớp dày đặc. Kiến trúc cố định và kích thước đầu vào rất nhỏ so với khối lượng công việc học máy thông thường, do đó, chuyển tiếp đơn giản là giải pháp lập trình cạnh tranh tự nhiên. Việc triển khai cũng tránh lưu trữ toàn bộ tập kiểm tra, chỉ giữ bộ nhớ tỷ lệ với mạng cố định và một hình ảnh. 

## Trường hợp thử nghiệm 

Bởi vì các ma trận mẫu chính thức bị bỏ qua trong câu lệnh được cung cấp nên chúng không thể được sao chép dưới dạng các xác nhận có thể thực thi được nếu không tạo ra dữ liệu tham số. Các thử nghiệm sau đây thực hiện việc thực hiện chuyển tiếp hoàn chỉnh với các khối tham số được tạo.```python
import sys
import io

def build_case():
    parts = []

    # Magic number.
    parts.append("0")

    # Conv1 weights: 4 x 25.
    for _ in range(4):
        parts.append(" ".join(["0"] * 25))

    # Conv1 bias.
    parts.append("0 0 0 0")

    # Conv2 weights: 9 x 36.
    for _ in range(9):
        parts.append(" ".join(["0"] * 36))

    # Conv2 bias.
    parts.append("0 0 0 0 0 0 0 0 0")

    # FC1 weights: 64 x 225.
    for _ in range(64):
        parts.append(" ".join(["0"] * 225))

    # FC1 bias.
    parts.append(" ".join(["0"] * 64))

    # FC2 weights: 4 x 64.
    for _ in range(4):
        parts.append(" ".join(["0"] * 64))

    # FC2 bias.
    parts.append("0 0 0 0")

    parts.append("1")

    # One all-zero 28 x 28 image.
    for _ in range(28):
        parts.append(" ".join(["0"] * 28))

    return "\n".join(parts) + "\n"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# All parameters and pixels are zero, so all four scores tie at zero.
assert run(build_case()) == "C\n", "all-zero tie case"

def build_bias_case():
    parts = []

    parts.append("0")

    for _ in range(4):
        parts.append(" ".join(["0"] * 25))
    parts.append("0 0 0 0")

    for _ in range(9):
        parts.append(" ".join(["0"] * 36))
    parts.append("0 0 0 0 0 0 0 0 0")

    for _ in range(64):
        parts.append(" ".join(["0"] * 225))
    parts.append(" ".join(["0"] * 64))

    for _ in range(4):
        parts.append(" ".join(["0"] * 64))

    # Class 3 has the largest final bias.
    parts.append("0 0 0 10")

    parts.append("1")
    for _ in range(28):
        parts.append(" ".join(["0"] * 28))

    return "\n".join(parts) + "\n"

assert run(build_bias_case()) == "U\n", "final bias case"

def build_negative_hidden_case():
    parts = []

    parts.append("0")

    # Conv1 produces zero everywhere.
    for _ in range(4):
        parts.append(" ".join(["0"] * 25))
    parts.append("0 0 0 0")

    # Conv2 produces zero everywhere.
    for _ in range(9):
        parts.append(" ".join(["0"] * 36))
    parts.append("0 0 0 0 0 0 0 0 0")

    # FC1 has negative bias, so ReLU makes every hidden value zero.
    for _ in range(64):
        parts.append(" ".join(["0"] * 225))
    parts.append(" ".join(["-5"] * 64))

    # FC2 has distinct biases.
    for _ in range(4):
        parts.append(" ".join(["0"] * 64))
    parts.append("1 2 3 4")

    parts.append("1")
    for _ in range(28):
        parts.append(" ".join(["0"] * 28))

    return "\n".join(parts) + "\n"

assert run(build_negative_hidden_case()) == "U\n", "ReLU before final layer"

def build_two_images_case():
    parts = []

    parts.append("0")

    for _ in range(4):
        parts.append(" ".join(["0"] * 25))
    parts.append("0 0 0 0")

    for _ in range(9):
        parts.append(" ".join(["0"] * 36))
    parts.append("0 0 0 0 0 0 0 0 0")

    for _ in range(64):
        parts.append(" ".join(["0"] * 225))
    parts.append("0 " * 63 + "0")

    for _ in range(4):
        parts.append(" ".join(["0"] * 64))
    parts.append("0 0 0 0")

    parts.append("2")

    for _ in range(28):
        parts.append(" ".join(["0"] * 28))

    for _ in range(28):
        parts.append(" ".join(["1"] * 28))

    return "\n".join(parts) + "\n"

assert run(build_two_images_case()) == "C\nC\n", "multiple images"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả các tham số và pixel bằng không |`C`| Xử lý ràng buộc và ReLU có giá trị bằng 0 | 
| Độ lệch cuối cùng`0 0 0 10`|`U`| argmax cuối cùng và ánh xạ lớp | 
| Thành kiến ​​FC1 âm |`U`| ReLU sau lớp dày đặc đầu tiên | 
| Hai hình ảnh liên tiếp |`C`,`C`| Đặt lại trạng thái trên mỗi hình ảnh và mức tiêu thụ đầu vào | 

## Vỏ cạnh 

Trường hợp hoàn toàn bằng 0 được xử lý bằng cách khởi tạo`best`về 0 và chỉ thay thế nó khi điểm sau đó lớn hơn hoàn toàn. Với bốn điểm cuối cùng bằng 0, vòng lặp không bao giờ thay đổi`best`, vì vậy đầu ra là`C`. Điều này quan trọng vì một quy tắc ràng buộc tùy ý có thể tạo ra một câu trả lời khác mặc dù mọi tính toán trên mạng đều đúng. 

Ranh giới tích chập được xử lý bằng cách lặp lại các vị trí hàng và cột của tích chập đầu tiên trên`range(24)`. Cửa sổ hợp lệ cuối cùng bắt đầu ở tọa độ 23 và bao gồm tọa độ từ 23 đến 27. Không có lần lặp nào bắt đầu ở 24, do đó việc triển khai không bao giờ đọc bên ngoài hình ảnh 28 x 28. Phép chập thứ hai sử dụng tương tự`range(10)`, với cửa sổ cuối cùng của nó bắt đầu ở tọa độ 9 và kết thúc ở vị trí 11 trong đầu vào 12 x 12. 

Kích thước kênh của tích chập thứ hai là một trường hợp cạnh khác. Đối với một kênh đầu ra, mã đọc vùng 3 x 3 từ kênh đầu vào 0, sau đó đọc vùng khác từ kênh 1, kênh 2 và kênh 3. Việc bỏ qua vòng lặp này sẽ coi tensor bốn kênh một cách hiệu quả là một kênh và sẽ tạo ra các điểm số hoàn toàn khác nhau. 

Thứ tự làm phẳng cũng rất đáng kể. Giả sử chín kênh gộp chứa các giá trị`0, 1, ..., 224`, với mỗi kênh chiếm 25 vị trí liên tiếp. Tế bào thần kinh dày đặc đầu tiên phải nhận 25 giá trị đầu tiên từ kênh 0, tiếp theo là 25 giá trị từ kênh 1, v.v. Bố cục lưu trữ được sử dụng bởi`features`chính xác là thứ tự đó, do đó ma trận dày đặc có thể được áp dụng mà không cần hoán vị bổ sung. 

Cuối cùng, quá trình triển khai sẽ xử lý từng hình ảnh từ đầu. Mảng tính năng và tích chập trung gian được phân bổ mới cho mỗi hình ảnh, vì vậy các giá trị từ một hình ảnh thử nghiệm không thể rò rỉ sang hình ảnh tiếp theo. Điều này đặc biệt dễ mắc sai lầm khi cố gắng tối ưu hóa phân bổ bằng cách sử dụng lại bộ đệm mà không ghi đè rõ ràng mọi vị trí.
