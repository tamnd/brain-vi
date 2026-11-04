---
title: "CF 1167D - RBS hai màu"
description: "Chúng tôi được cung cấp một chuỗi dấu ngoặc đơn hợp lệ. Hãy coi nó như một bước đi bắt đầu ở độ cao 0, trong đó mỗi dấu ngoặc mở sẽ tăng chiều cao lên 1 và mỗi dấu ngoặc đóng sẽ giảm chiều cao đi 1 và bước đi không bao giờ âm và kết thúc ở 0."
date: "2026-06-13T08:58:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1167
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 65 (Rated for Div. 2)"
rating: 1500
weight: 1167
solve_time_s: 159
verified: true
draft: false
---

[CF 1167D - RBS hai màu](https://codeforces.com/problemset/problem/1167/D) 

**Đánh giá:** 1500 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi dấu ngoặc đơn hợp lệ. Hãy coi nó như một bước đi bắt đầu ở độ cao 0, trong đó mỗi dấu ngoặc mở sẽ tăng chiều cao lên 1 và mỗi dấu ngoặc đóng sẽ giảm chiều cao đi 1 và bước đi không bao giờ âm và kết thúc ở 0. 

Nhiệm vụ là chia cấu trúc cân bằng đơn này thành hai chuỗi con, một chuỗi được đánh dấu bằng 0 và chuỗi còn lại được đánh dấu bằng 1 mà không thay đổi thứ tự. Bản thân mỗi dãy con phải là một dãy dấu ngoặc đơn cân bằng hợp lệ. Vì vậy, sau khi chỉ lọc các ký tự có nhãn 0, chúng ta vẫn phải có cấu trúc ngoặc đúng và tương tự phải giữ cho nhãn 1. 

Trong số tất cả các phân tách hợp lệ, chúng tôi muốn giảm thiểu độ sâu lồng ghép lớn hơn của hai chuỗi kết quả. Độ sâu lồng nhau tương ứng với chiều cao tối đa đạt được trong chế độ xem tổng tiền tố của chính chuỗi đó. 

Ràng buộc n lên tới 2×10^5 ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các phân vùng hoặc thực hiện xác thực lồng nhau trên mỗi phần phân chia. Mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính, vì O(n log n) có thể chấp nhận được nhưng O(n^2) thì không. 

Một chế độ thất bại tinh vi xuất hiện khi người ta cố gắng gán các dấu ngoặc một cách tham lam chỉ dựa trên các quyết định cục bộ như “màu thay thế” hoặc “phân chia theo vị trí tương đương”. Ví dụ: trong chuỗi “((()))”, các màu xen kẽ tạo ra màu đỏ “(()” và màu xanh lam “(()”, cả hai đều không hợp lệ vì không giữ được sự cân bằng. Một ý tưởng ngây thơ khác là luôn gán theo độ tương đương độ sâu hiện tại, nhưng nếu không kiểm soát tính nhất quán của cấu trúc đẩy/pop, một màu có thể trở nên mất cân bằng mặc dù tổng trình tự vẫn ổn. 

Khó khăn chính là tính hợp lệ phụ thuộc vào việc khớp mở và đóng một cách nhất quán trong mỗi màu chứ không chỉ cấu trúc tổng thể. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là gán cho mỗi khung một trong hai màu theo mọi cách có thể và kiểm tra tính hợp lệ của cả hai chuỗi cảm ứng. Có 2^n phép gán và đối với mỗi phép gán, chúng ta cần xác thực tuyến tính hai chuỗi dấu ngoặc, dẫn đến O(n·2^n), vượt xa giới hạn. 

Chúng ta cần khai thác cấu trúc của chuỗi ngoặc thông thường. Quan sát quan trọng là mọi dấu ngoặc mở có thể được ghép với chính xác một dấu ngoặc đóng trong một khớp chuẩn và các cặp này tạo thành cấu trúc lồng giống như cây. 

Cái nhìn sâu sắc quan trọng là diễn giải trình tự như một bước đi sâu. Tại mỗi vị trí, chúng ta biết được độ sâu hiện tại. Vấn đề giảm xuống còn việc chia tất cả các dấu ngoặc thành hai ngăn sao cho mỗi ngăn xếp độc lập tạo thành một bước đi sâu hợp lệ. Cách rõ ràng để đảm bảo tính hợp lệ là đảm bảo rằng đối với mỗi màu, mọi tiền tố không bao giờ âm, tương đương với việc đảm bảo rằng mọi phần mở được gán cho một màu cuối cùng sẽ khớp với phần đóng cùng màu theo đúng thứ tự. 

Điều này gợi ý việc kiểm soát việc phân công bằng cách sử dụng tính chẵn lẻ của độ sâu. Khi duyệt chuỗi, chúng tôi theo dõi độ sâu hiện tại trước khi xử lý từng ký tự. Nếu chúng ta luân phiên gán dựa trên độ sâu là số lẻ hay số chẵn thì tất cả các dấu ngoặc ở cùng mức độ lồng nhau sẽ có cùng một màu. Vì cấu trúc lồng nhau được căn chỉnh hoàn hảo với độ tương đương độ sâu nên mỗi màu kế thừa cấu trúc không giao nhau hợp lệ. 

Điều này hoạt động vì độ sâu thay đổi theo cách có cấu trúc: mỗi “(” tăng độ sâu trước khi khớp với “)” làm giảm độ sâu, do đó cả hai điểm cuối của một cặp khớp đều có cùng độ sâu tương đương ngay trước quyết định khung đóng. Do đó cả hai đầu đều có màu giống hệt nhau, giữ được sự cân bằng. 

Chúng ta vẫn có quyền tự do hoán đổi ánh xạ chẵn lẻ thành màu nào, do đó, độ sâu tối đa giữa cả hai chuỗi được giảm thiểu bằng cách cân bằng việc phân bổ giữa các lớp chẵn và lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Độ sâu tương đương tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét chuỗi trong khi duy trì độ sâu hiện tại.

1. Khởi tạo độ sâu = 0 và một mảng kết quả trống. 

Độ sâu thể hiện mức độ lồng sâu của chúng ta trước khi đọc từng ký tự. 
2. Với mỗi ký tự trong chuỗi: 

Chúng tôi quyết định màu sắc của nó dựa trên độ tương đương độ sâu hiện tại. 
3. Nếu ký tự là “(”, ta gán màu = độ sâu % 2 thì tăng độ sâu lên 1. 

Điều này đảm bảo tất cả các lỗ mở ở cùng cấp độ cấu trúc đều thuộc cùng một nhóm. 
4. Nếu ký tự là “)”, đầu tiên chúng ta giảm độ sâu đi 1, sau đó gán màu = độ sâu % 2. 

Điều quan trọng là dấu ngoặc đóng khớp với độ sâu của cấu trúc mở tương ứng của nó, để cả hai điểm cuối đều nhận được cùng một màu. 
5. Lưu trữ màu được chỉ định trong chuỗi đầu ra. 
6. Sau khi xử lý tất cả các ký tự, xuất ra chuỗi màu đã xây dựng. 

Tại sao nó hoạt động: mỗi cặp dấu ngoặc đơn khớp tương ứng với một đoạn của bước đi theo độ sâu bắt đầu ở độ sâu d nào đó và quay trở lại từ d+1 trở lại d. Cả hai điểm cuối đều được gán bằng cách sử dụng cùng một giá trị chẵn lẻ độ sâu d, do đó cả hai đầu đều có cùng màu. Điều này đảm bảo rằng mọi cặp so khớp đều được chứa hoàn toàn trong một dãy con. Vì việc lồng nhau được giữ nguyên trong các lớp chẵn lẻ nên mỗi lớp màu tạo thành một RBS hợp lệ. Không có sự xen kẽ không hợp lệ nào có thể xảy ra vì phép gán dựa trên độ sâu tôn trọng cấu trúc ngăn xếp tự nhiên của việc so khớp. 

## Giải pháp Python```
PythonRun
```Giải pháp dựa hoàn toàn vào độ sâu theo dõi dưới dạng dấu vân tay cấu trúc của cây khung. Điểm tinh tế duy nhất là thứ tự các thao tác để đóng dấu ngoặc: độ sâu phải giảm trước khi tính chẵn lẻ, nếu không, dấu ngoặc đóng sẽ được gán cho lớp sai và phá vỡ tính nhất quán ghép nối. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào “()()”. 

Chúng tôi theo dõi độ sâu và nhiệm vụ. 

| tôi | char | độ sâu trước | hành động | độ sâu sau | màu sắc | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ( | 0 | gán 0, inc | 1 | 0 | 
| 1 | ) | 1 | tháng 12, gán 0 | 0 | 0 | 
| 2 | ( | 0 | gán 0, inc | 1 | 0 | 
| 3 | ) | 1 | tháng 12, gán 0 | 0 | 0 | 

Tất cả các ký tự chuyển sang một màu, tạo ra một RBS duy nhất hợp lệ và một màu trống thứ hai. 

Bây giờ hãy xem xét “(())”. 

| tôi | char | độ sâu trước | hành động | độ sâu sau | màu sắc | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ( | 0 | gán 0, inc | 1 | 0 | 
| 1 | ( | 1 | gán 1, inc | 2 | 1 | 
| 2 | ) | 2 | tháng 12, giao 1 | 1 | 1 | 
| 3 | ) | 1 | tháng 12, gán 0 | 0 | 0 | 

Chúng ta nhận được hai chuỗi: một chuỗi chứa các dấu ngoặc của lớp bên ngoài và một chuỗi chứa các dấu ngoặc của lớp bên trong. Mỗi cái được cân bằng độc lập vì các cặp phù hợp có cùng màu được chỉ định. 

Điều này chứng tỏ mức độ lồng nhau được phân tách rõ ràng bằng tính chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | truyền một lần qua chuỗi | 
| Không gian | O(n) | lưu trữ chuỗi màu đầu ra | 

Thuật toán xử lý mỗi ký tự một lần và thực hiện công việc liên tục trên mỗi bước. Với n lên tới 2×10^5, điều này phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`()`|`11`| trường hợp hợp lệ nhỏ nhất | 
|`(())`| chia cân bằng | tách tổ | 
|`()()`| phân công thống nhất hoặc ổn định | cấu trúc tuần tự | 
|`((()))`| xen kẽ các lớp | hành vi làm tổ sâu | 

## Vỏ cạnh 

Vỏ một cạnh là một cấu trúc hoàn toàn phẳng giống như “()()()”. Trong trường hợp này, tính chẵn lẻ độ sâu không bao giờ thay đổi theo cách tạo ra sự tách biệt, do đó tất cả các dấu ngoặc có thể kết thúc trong một nhóm. Thuật toán xử lý việc này một cách chính xác vì cho phép một RBS hợp lệ duy nhất và mang lại độ sâu lồng nhau là 1 cho nhóm trống còn lại. 

Một trường hợp cạnh khác là lồng tối đa như “((((...))))”. Ở đây, mọi cấp độ đều thay đổi màu sắc một cách nhất quán và mỗi màu tạo thành một chuỗi độc lập được lồng vào nhau một cách hoàn hảo. Phép gán dựa trên độ sâu đảm bảo rằng không có cặp nào bị phân chia theo màu sắc, vì cả hai điểm cuối của mỗi cặp đều có chung phép gán chẵn lẻ giống hệt nhau bắt nguồn từ quá trình chuyển đổi độ sâu cấu trúc của chúng.
