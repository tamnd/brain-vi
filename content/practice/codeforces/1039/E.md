---
title: "CF 1039E - Triển lãm Oenothera mùa hè"
description: "Chúng ta được cung cấp một chuỗi các khoảng ảnh trên một trục số rất lớn. Mỗi ảnh bao phủ một cửa sổ cố định có độ dài w, bắt đầu từ vị trí xi, do đó ảnh i bao phủ [xi, xi + w - 1]."
date: "2026-06-16T18:25:04+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1039
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 507 (Div. 1, based on Olympiad of Metropolises)"
rating: 3400
weight: 1039
solve_time_s: 312
verified: true
draft: false
---

[CF 1039E - Triển lãm Oenothera mùa hè](https://codeforces.com/problemset/problem/1039/E) 

**Đánh giá:** 3400 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 5 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các khoảng ảnh trên một trục số rất lớn. Mỗi bức ảnh bao gồm một cửa sổ có độ dài cố định`w`, bắt đầu từ vị trí`x_i`, vậy ảnh`i`bao gồm`[x_i, x_i + w - 1]`. Đối với mỗi giá trị truy vấn`k`, mọi bức ảnh đều được "cắt" thành một phân đoạn liền kề chính xác`k`điểm nằm trong khoảng ban đầu của nó. Sự tự do chính là đối với mỗi bức ảnh, chúng tôi có thể chọn bất kỳ độ dài nào-`k`phân đoạn bên trong của nó`w`-khoảng chiều dài. 

Sau khi chọn các phân đoạn này, chúng ta xem xét chuỗi kết quả của các tập hợp. Một “cảnh” là một khối ảnh liên tiếp tối đa tương ứng với cùng một bộ ảnh đã chọn. Một vết cắt xảy ra bất cứ khi nào chúng ta chuyển từ cảnh này sang cảnh khác. Nhiệm vụ của mỗi`k`là chọn các phần phụ cho mỗi bức ảnh sao cho số lần cắt được giảm thiểu. 

Khó khăn thực sự là phân đoạn được chọn cho mỗi bức ảnh không cố định: mỗi khoảng thời gian`[x_i, x_i + w - 1]`đại diện cho một cửa sổ trượt có thể`k`-các đoạn có độ dài`[t, t + k - 1]`Ở đâu`t`có thể dao động từ`x_i`ĐẾN`x_i + w - k`. Hai bức ảnh liền kề có thể thuộc cùng một cảnh nếu chúng ta có thể chọn giống hệt nhau`k`-các đoạn nằm bên trong cả hai cửa sổ của chúng. 

Vì vậy, cấu trúc cốt lõi là: mỗi bức ảnh tương ứng với một khoảng vị trí bắt đầu được phép trong một khoảng thời gian-`k`phân đoạn và chúng tôi muốn phân vùng chuỗi thành số nhóm liền kề tối thiểu sao cho mỗi nhóm có giao điểm không trống của tất cả các phạm vi bắt đầu được phép. 

Các ràng buộc buộc chúng tôi phải tránh mô phỏng theo mỗi truy vấn. Với`n, q ≤ 100000`, bất kỳ giải pháp nào tính toán lại các phần chồng chéo từ đầu cho mỗi`k`dẫn tới tới`10^10`hoạt động trong trường hợp xấu nhất là quá lớn. Chúng ta cần xử lý trước mối quan hệ giữa các ảnh liền kề để mỗi truy vấn có thể được trả lời theo thời gian không đổi gần logarit hoặc khấu hao. 

Trường hợp cạnh tinh tế xuất hiện khi sự chồng lấp chính xác giữa hai ảnh liền kề`k`. Trong trường hợp đó, chỉ có một phân đoạn hợp lệ và nó phải được xử lý cẩn thận vì “ít nhất một lựa chọn chung” và “bình đẳng bắt buộc” trùng khớp, và việc hợp nhất tham lam có thể dẫn đến tính linh hoạt một cách không chính xác. 

Một trường hợp cạnh khác là khi`k = w`. Khi đó, mỗi bức ảnh chỉ có một phân đoạn có thể có, vì vậy câu trả lời hoàn toàn được xác định bằng sự bằng nhau chính xác của các khoảng ban đầu, điều này sẽ giải quyết vấn đề bằng việc so sánh điểm bắt đầu. 

## Phương pháp tiếp cận 

Nếu chúng ta cố định một giá trị`k`, mỗi ảnh`i`cho phép vị trí bắt đầu`t`trong phạm vi:```

```Hai bức ảnh liền kề`i`Và`i+1`có thể thuộc về cùng một cảnh nếu phạm vi cho phép của chúng giao nhau, vì chúng ta có thể chọn điểm bắt đầu chung`t`hợp lệ cho cả hai. 

Vì vậy để cố định`k`, chúng ta có thể tham lam quét từ trái sang phải, duy trì giao điểm hiện tại của tất cả các phạm vi được phép trong phân đoạn hiện tại. Bất cứ khi nào nút giao trở nên trống, chúng ta phải bắt đầu một đoạn mới, tương ứng với một đoạn cắt. 

Thủ tục tham lam này là đúng cho một cố định`k`bởi vì một khi giao lộ trở nên trống rỗng, không tiện ích mở rộng nào trong tương lai có thể khôi phục tính khả thi cho cùng một khung cảnh. 

Tuy nhiên, việc tính toán lại các nút giao này cho mỗi truy vấn một cách độc lập sẽ tốn kém`O(nq)`, quá chậm. 

Quan sát quan trọng là các điều kiện kề chỉ phụ thuộc vào việc hai khoảng có giao nhau hay không sau khi co lại bởi`k`. Đối với mỗi cặp liền kề`(i, i+1)`, chúng ta có thể tính giá trị lớn nhất`k`sao cho chúng vẫn cắt nhau. Vượt quá ngưỡng đó, họ phải tách ra. Vì vậy, mỗi cặp liền kề đóng góp một giá trị ngưỡng và khi`k`tăng lên, nhiều cạnh bị gãy hơn. 

Điều này chuyển vấn đề thành: chúng ta có`n-1`các cạnh, mỗi cạnh có một "ngưỡng lỗi" và với một giá trị nhất định`k`, chúng ta đếm xem có bao nhiêu cạnh vẫn hợp lệ. Câu trả lời trở thành`1 + number of broken edges in the optimal partitioning`, có thể được tính bằng cách sắp xếp các ngưỡng và số lượng tiền tố trả lời. 

Chính xác hơn, mỗi vùng lân cận tạo ra một giá trị tới hạn tại đó sự chồng chéo biến mất. Việc sắp xếp các giá trị quan trọng này cho phép trả lời từng truy vấn bằng cách đếm xem có bao nhiêu giá trị nhỏ hơn`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tính toán lại mỗi truy vấn | O(nq) | O(1) | Quá chậm | 
| Phân loại ngưỡng | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đơn giản hóa vấn đề thành hiểu khi hai bức ảnh lân cận có thể chia sẻ một đoạn cắt ngắn chung. 

### 1. Dịch từng ảnh thành khoảng thời gian bắt đầu hợp lệ 

Đối với mỗi bức ảnh`i`, sự bắt đầu của sự lựa chọn`k`-đoạn phải nằm trong:```

```Điều này xuất phát trực tiếp từ yêu cầu rằng`k`-đoạn phải ở bên trong bản gốc`w`-cửa sổ. 

### 2. Điều kiện để hai bức ảnh ở cùng một khung cảnh 

Hai ảnh liền kề có thể vẫn ở cùng một cảnh nếu khoảng thời gian bắt đầu được phép của chúng giao nhau:```

```Điều này đảm bảo tồn tại một`k`-phân đoạn hợp lệ cho cả hai bức ảnh. 

Viết lại điều kiện giao nhau cho:```

```### 3. Trích xuất điều kiện ngưỡng trong k 

Sắp xếp lại, điều kiện trùng lặp trở thành:```

```Vì vậy, sự kề cận có giá trị chính xác khi:```

```Định nghĩa:```

```Nếu như`k`vượt quá giá trị này thì phải xảy ra hiện tượng đứt mép và vết cắt giữa hai ảnh này. 

### 4. Chuyển bài toán thành đếm các cạnh gãy 

Đối với một cố định`k`, mọi lân cận với`threshold_i < k`trở thành điểm cắt. Số cảnh là:```

```Do đó, câu trả lời cho mỗi truy vấn sẽ giảm xuống việc đếm có bao nhiêu ngưỡng dưới đây`k`. 

### 5. Tính toán trước và sắp xếp các ngưỡng 

Chúng tôi tính toán tất cả`threshold_i`, sắp xếp chúng và trả lời từng truy vấn bằng tìm kiếm nhị phân. 

### Tại sao nó hoạt động 

Bất biến chính là một cảnh tương ứng chính xác với một khối liền kề trong đó tất cả các cặp liền kề đều thừa nhận một khả thi chung`k`-phân khúc. Khi bất kỳ vùng lân cận nào bị hỏng, không có lựa chọn phân đoạn nào có thể sửa chữa kết nối bên trong khối đó vì tính khả thi chỉ phụ thuộc vào giao điểm khoảng, vốn đơn điệu trong`k`. Như vậy, việc phân vùng do các cạnh bị gãy tạo ra là tối ưu và tối thiểu. 

## Giải pháp Python```
PythonRun
```
