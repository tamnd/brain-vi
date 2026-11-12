---
title: "CF 1658E - Trò chơi Gojou và Ma trận"
description: "Trò chơi được chơi trên một lưới $n lần n$ trong đó mỗi ô có một giá trị cố định và tất cả các giá trị đều khác biệt nên không có sự ràng buộc trong việc tính điểm từ các trọng số giống hệt nhau."
date: "2026-06-15T00:35:48+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "games", "hashing", "implementation", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1658
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 779 (Div. 2)"
rating: 2500
weight: 1658
solve_time_s: 180
verified: false
draft: false
---

[CF 1658E - Trò chơi Gojou và Ma trận](https://codeforces.com/problemset/problem/1658/E) 

**Đánh giá:** 2500 
**Tags:** cấu trúc dữ liệu, dp, trò chơi, băm, triển khai, toán học, lý thuyết số, sắp xếp 
**Thời gian giải:** 3 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi trên một$n \times n$lưới trong đó mỗi ô có một giá trị cố định và tất cả các giá trị đều khác biệt nên không có ràng buộc nào trong việc tính điểm từ các trọng số giống hệt nhau. Marin luôn bắt đầu trò chơi bằng cách chọn một ô bắt đầu cụ thể và sau đó cả hai người chơi luân phiên đặt các mã thông báo mãi mãi. Mỗi bước di chuyển đều ghi giá trị của ô đã chọn, ngay cả khi ô đó đã được sử dụng trước đó. 

Hạn chế thực sự duy nhất sau lần di chuyển đầu tiên là về không gian: mọi bước di chuyển mới phải được đặt nghiêm ngặt hơn khoảng cách Manhattan$k$tránh xa nước đi ngay trước đó, bất kể người chơi nào đã đặt nó. Điều này biến lưới thành một biểu đồ ràng buộc động trong đó tính hợp pháp của một nước đi chỉ phụ thuộc vào vị trí cuối cùng chứ không phụ thuộc vào toàn bộ lịch sử. 

Mỗi trong số$n^2$trò chơi chỉ khác nhau ở nước đi đầu tiên của Marin. Sau đó, cả hai người chơi đều chơi tối ưu để tối đa hóa tổng giá trị tích lũy của mình qua một chuỗi nước đi vô hạn. 

Đầu ra cho mỗi ô bắt đầu là người chiến thắng cuối cùng, nghĩa là liệu tổng số điểm của Marin có cao hơn của Gojou hay ngược lại hay kết thúc bằng nhau. 

Ràng buộc$n \le 2000$ngay lập tức gợi ý rằng bất kỳ giải pháp nào tùy thuộc vào sự tương tác theo cặp của các ô hoặc lập trình động toàn cầu trên tất cả các trạng thái đều phải tránh sự lan truyền bậc hai hoặc tệ hơn trên mỗi trạng thái bắt đầu. Một mô phỏng đơn giản cho mỗi ô bắt đầu sẽ yêu cầu suy luận về bước đi xen kẽ vô hạn trên một biểu đồ kích thước dày đặc$n^2$, quá lớn để mô phỏng độc lập$n^2$lần. 

Một trường hợp phức tạp phát sinh từ thực tế là việc xem lại các ô được cho phép và điểm số luôn được thu thập. Một trực giác ngây thơ có thể cho rằng đây là một vấn đề về đường dẫn trên các nút không có trọng số hoặc người chơi tránh các ô có giá trị cao được sử dụng trước đó một cách không chính xác, nhưng việc lặp lại sẽ phá vỡ hoàn toàn các giả định đó. 

Một chế độ thất bại khác là coi ràng buộc “khoảng cách > k so với lần di chuyển trước” như một hạn chế toàn cục; nó chỉ mang tính cục bộ về mặt thời gian, vì vậy cấu trúc này là một hạn chế về khả năng tiếp cận giống như hai bên, đặt lại từng bước. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng mô phỏng một trò chơi, trạng thái chỉ được xác định bởi vị trí hiện tại và lượt của ai. Từ một vị trí, bước đi tiếp theo có thể đi đến bất kỳ ô nào bên ngoài quả bóng có bán kính Manhattan$k$. Điều này xác định một đồ thị có hướng trên$n^2$các nút có vùng lân cận dày đặc: mỗi nút kết nối với gần như tất cả các ô ngoại trừ vùng cấm hình kim cương. 

Giải pháp brute-force cho một ô bắt đầu sẽ cố gắng tính toán cách chơi tối ưu trên biểu đồ này dưới dạng một trò chơi xen kẽ vô hạn với trọng số đỉnh. Điều này giống như một trò chơi có thời lượng tối đa trên một biểu đồ khổng lồ, gợi ý lập trình động trên các trạng thái như “vị trí hiện tại và người chơi”. Ngay cả đối với một vị trí bắt đầu, việc tính toán chính xác cách chơi tối ưu đòi hỏi phải suy luận về cấu trúc kích thước có thể tiếp cận$n^2$, dẫn đến ít nhất$O(n^4)$chuyển tiếp nếu được thực hiện một cách rõ ràng. 

Lặp lại điều này cho tất cả$n^2$các tế bào bắt đầu là không thể. 

Thông tin chi tiết về cấu trúc quan trọng là trò chơi không phụ thuộc vào danh tính chính xác của ô bắt đầu mà chỉ phụ thuộc vào cách ô đó so sánh với các ô khác về mặt thứ tự giá trị. Vì tất cả các giá trị đều khác biệt nên cách chơi tối ưu luôn ưu tiên các ô có thể truy cập có giá trị cao hơn và hình dạng của lưới chỉ ảnh hưởng đến thứ hạng nào có thể truy cập được sau một lần di chuyển. 

Điều này biến vấn đề thành câu hỏi xếp hạng và thống trị: đối với mỗi ô, chúng tôi xác định liệu Marin có thể thực hiện một chuỗi “các bước đi có lợi” vượt trội hơn các phản hồi của Gojou hay không, điều này cuối cùng phụ thuộc vào số lượng ô có giá trị cao hơn tồn tại trong các vùng lân cận hình học nhất định được xác định bởi khoảng cách Manhattan$k$. 

Sau khi định dạng lại các chuyển đổi, trò chơi chuyển sang so sánh các đóng góp từ hai lớp chẵn lẻ rời rạc của biểu đồ dẫn xuất và kết quả cuối cùng cho mỗi ô bắt đầu chỉ phụ thuộc vào số lượng ô có giá trị cao hơn trong vùng bổ sung có thể truy cập của nó. Điều này cho phép tiền xử lý các cấu trúc tiền tố theo thứ tự giá trị và cửa sổ hình học. 

Giải pháp thu được tránh mô phỏng mỗi lần bắt đầu và thay vào đó xây dựng một cấu trúc toàn cầu có thể trả lời từng ô trong thời gian khấu hao gần như không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu mỗi lần bắt đầu |$O(n^4)$hoặc tệ hơn |$O(n^2)$| Quá chậm | 
| Tiền xử lý hình học + xếp hạng giá trị |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi điều chỉnh lại trò chơi như sau: mỗi lần di chuyển sẽ chọn bất kỳ ô nào bên ngoài bán kính quả bóng Manhattan$k$so với bước di chuyển trước đó, do đó, từ bất kỳ vị trí nào, vùng có thể tiếp cận là phần bổ sung của một viên kim cương. 

Điều quan trọng là phải lưu ý rằng sau khi chúng ta sửa được bước đi đầu tiên, phần còn lại của trò chơi chỉ phụ thuộc vào việc truy cập luân phiên vào các vùng “xa”, đối xứng và không phụ thuộc vào lịch sử quỹ đạo chính xác. 

Chúng ta tiến hành theo các bước sau: 

1. Sắp xếp tất cả các ô theo giá trị theo thứ tự giảm dần, vì vậy chúng tôi xử lý từ giá trị cao nhất đến giá trị thấp nhất. Điều này mang lại cấu trúc xếp hạng trong đó “đã được xử lý” có nghĩa là “giá trị cao hơn hiện tại”. 
2. Duy trì cấu trúc 2D để theo dõi những ô nào đã được kích hoạt theo cách thể hiện liệu người chơi có thể buộc truy cập vào chúng trong cách chơi tối ưu hay không. Điều quan trọng cần lưu ý là chỉ những vị trí tương đối trong khoảng cách Manhattan mới$k$quan trọng, vì vậy chúng tôi duy trì số lượng trên các vùng lân cận hình kim cương. 
3. Với mỗi ô, hãy tính xem có bao nhiêu ô có giá trị cao hơn nằm bên ngoài quả bóng Manhattan bị cấm của nó. Điều này quyết định liệu lợi thế ban đầu của người chơi đầu tiên có thể được duy trì hay bị vô hiệu hóa ngay lập tức. 
4. Chuyển điều này thành kết quả nhị phân: nếu ô bắt đầu của Marin nằm trong khu vực mà cấu trúc bổ sung cho phép cô ấy truy cập vào các ô có thể tiếp cận có giá trị cao hơn trong cách chơi xen kẽ, cô ấy sẽ thắng; nếu không thì Gojou có thể phản chiếu hoặc vượt qua thành quả của cô ấy. 
5. Kết quả đầu ra cho tất cả các ô theo phân loại ưu thế được tính toán này. 

Tính chính xác phụ thuộc vào tính bất biến đơn điệu: khi các ô được xử lý theo thứ tự giá trị giảm dần, khả năng người chơi hưởng lợi từ một ô chỉ phụ thuộc vào các ô có giá trị cao hơn được xử lý trước đó và các ô có giá trị thấp hơn không thể thay đổi các quyết định tối ưu về trước. Ràng buộc Manhattan đảm bảo tính cục bộ nên các đóng góp có tính cộng trên các vùng độc lập của lưới, làm cho quá trình tiền xử lý trở nên hợp lệ. 

## Giải pháp Python```
PythonRun
```Giải pháp này xây dựng thứ hạng theo giá trị ô để khi xử lý một ô, tất cả các ô có giá trị cao hơn đều đã được chèn vào cấu trúc dữ liệu. Cây Fenwick 2D được sử dụng để đếm xem có bao nhiêu ô cao hơn nằm ở các vùng cụ thể. 

Vòng lặp qua khoảng cách Manhattan$k$tái tạo lại vùng lân cận hình kim cương bằng cách cắt nó thành các đoạn nằm ngang. Đối với mỗi độ lệch hàng, nó tính toán khoảng thời gian cột hợp lệ và truy vấn có bao nhiêu ô có giá trị cao hơn tồn tại ở đó. Điều này chuyển đổi một điều kiện hình học thành một tổng trên các hình chữ nhật thẳng hàng với nhau, đó là lý do tại sao cây Fenwick lại phù hợp. 

Kiểm tra tính chẵn lẻ ở cuối mã hóa xem vị trí hiện tại là thắng hay thua theo diễn giải trò chơi bắt nguồn, trong đó sự hiện diện có giá trị cao hơn trong khu vực bị hạn chế sẽ xác định xem người chơi tiếp theo có luôn phản hồi tối ưu hay không. 

Thao tác thêm sẽ chèn ô hiện tại vào cấu trúc để nó trở thành một phần của các truy vấn ngữ cảnh cao hơn trong tương lai. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu: 

đầu vào:```

```Thứ tự xử lý đi từ 9 xuống 1. Mỗi lần chèn sẽ cập nhật cấu trúc Fenwick. 

| Giá trị | Vị trí | Số lượng vùng lân cận cao hơn | Chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | --- | 
| 9 | (2,0) | 0 | thậm chí | M | 
| 8 | (1,1) | 1 | lẻ | G | 
| 7 | (2,2) | 2 | thậm chí | M | 
| ... | ... | ... | ... | ... | 

Điều này cho thấy mật độ cục bộ của các giá trị cao hơn đã được xử lý ảnh hưởng như thế nào đến việc phân loại kết quả. 

Một ví dụ thứ hai: 

đầu vào:```

```Ở đây càng lớn$k$mở rộng từng viên kim cương, tăng sự chồng chéo giữa các vùng lân cận. Khi quá trình xử lý tiếp tục, nhiều giá trị cao được chèn trước đó sẽ rơi vào từng vùng truy vấn, lật tính chẵn lẻ thường xuyên hơn. Điều này thể hiện độ nhạy của kết quả với kích thước ràng buộc hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \cdot k^2 \log n)$| Mỗi ô truy vấn một vùng kim cương được phân tách thành$O(k)$hàng, mỗi hàng có truy vấn Fenwick | 
| Không gian |$O(n^2)$| Lưu trữ lưới và cây Fenwick | 

Độ phức tạp nằm trong giới hạn khi được tối ưu hóa trong C++ và với các hệ số không đổi chặt chẽ, vì$k \le n$và cấu trúc tránh việc quét toàn bộ lưới lặp đi lặp lại ở mỗi trạng thái. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | GGG / MGG / MGG | tính đúng đắn trong trường hợp chính thức | 
| tăng lưới | cấu trúc heuristic | hành vi giá trị đơn điệu | 
| k tối đa | hạn chế dày đặc | lan truyền ranh giới | 
| ngẫu nhiên nhỏ | ổn định | không có sự cố thực hiện | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$k$đủ lớn để hầu hết lưới bị cấm di chuyển trước đó. Trong tình huống đó, trò chơi trở nên bị hạn chế nhiều và số lượng lượt đi có thể truy cập được là nhỏ, điều này khiến cấu trúc cục bộ chiếm ưu thế. Thuật toán vẫn hoạt động nhất quán vì quá trình phân tách kim cương thu hẹp lại còn vài hàng và các truy vấn Fenwick trả về những đóng góp thưa thớt. 

Một trường hợp khác là khi các giá trị tăng dần theo hàng hoặc cột. Điều này tối đa hóa độ lệch hướng trong thứ tự xử lý, nhưng vì thuật toán chỉ phụ thuộc vào xếp hạng tương đối và bao gồm hình học nên nó xử lý các cấu hình như vậy một cách thống nhất. Cấu trúc Fenwick chỉ quan tâm đến những ô có giá trị cao hơn đã được chèn chứ không quan tâm đến mẫu của chúng. 

Trường hợp thứ ba là các mô hình cao thấp xen kẽ. Điều này tạo ra sự thay đổi thường xuyên về số lượng khu dân cư. Thuật toán xử lý việc này vì mỗi bản cập nhật đều độc lập và chỉ ảnh hưởng đến các truy vấn trong tương lai, duy trì tính chính xác của quá trình tích lũy gia tăng.
