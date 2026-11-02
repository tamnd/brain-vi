---
title: "CF 1085D - Cây có đường kính tối thiểu"
description: "Chúng tôi được cấp một cái cây và tổng số tiền cố định trong “ngân sách trọng lượng” $s$. Mỗi cạnh phải được gán một trọng số thực không âm và tổng trên tất cả các cạnh phải bằng chính xác $s$."
date: "2026-06-15T14:44:21+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1085
codeforces_index: "D"
codeforces_contest_name: "Technocup 2019 - Elimination Round 4"
rating: 1700
weight: 1085
solve_time_s: 140
verified: false
draft: false
---

[CF 1085D - Cây có đường kính tối thiểu](https://codeforces.com/problemset/problem/1085/D) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, triển khai, cây 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một cái cây và tổng số tiền cố định “ngân sách trọng lượng”$s$. Mỗi cạnh phải được gán một trọng số thực không âm và tổng trên tất cả các cạnh phải bằng chính xác$s$. Sau khi đã gán trọng số, khoảng cách giữa hai đỉnh là tổng trọng số của các cạnh dọc theo đường đi duy nhất của chúng trong cây và đường kính là khoảng cách tối đa như vậy trên tất cả các cặp đỉnh. 

Nhiệm vụ là phân phối tổng trọng số trên các cạnh sao cho đường dẫn kiểu từ gốc đến lá dài nhất ở bất kỳ vị trí nào trong cây trở nên nhỏ nhất có thể. 

Khó khăn chính là chúng ta không chọn một con đường mà phân bổ khối lượng trên toàn cầu qua cấu trúc cây. Bất kỳ cạnh nào chúng ta tạo ra nặng nề đều làm tăng tất cả các đường đi qua nó, do đó, cấu hình tối ưu hoàn toàn phụ thuộc vào số lượng “hướng quan trọng” mà cây có, nghĩa là có bao nhiêu nhánh cạnh tranh để tạo thành các đường đi dài. 

Các ràng buộc cho phép lên đến$10^5$các nút, do đó, bất kỳ giải pháp nào về cơ bản đều phải tuyến tính hoặc gần tuyến tính theo kích thước của cây. Một bậc hai hoặc thậm chí$O(n \log^2 n)$việc xây dựng sẽ gặp rủi ro nếu nó liên quan đến việc tính toán lại toàn cầu lặp đi lặp lại. Điều này gợi ý rõ ràng rằng câu trả lời chỉ phụ thuộc vào một tham số cấu trúc nhỏ của cây chứ không phụ thuộc vào bất kỳ cấu hình chi tiết nào của các trọng số. 

Một số trường hợp đặc biệt bộc lộ những gì không tầm thường. 

Nếu cây là một đường đi đơn giản thì không có sự phân nhánh. Bất kỳ sự phân bố trọng số nào cũng vẫn tạo ra một đường dẫn duy nhất có tổng trọng số luôn là$s$, do đó đường kính buộc phải là$s$. Một nỗ lực ngây thơ nhằm “phân bổ sức nặng” không thể làm giảm được nó. 

Nếu cái cây là một ngôi sao thì mọi con đường đều đi qua trung tâm. Có thể chia tổng trọng số thành nhiều cạnh, do đó không có đường đi nào tích lũy nhiều trọng số. Ví dụ, với$n=4$và một ngôi sao có tâm ở số 1, gán trọng số bằng nhau$s/3$sản lượng đường kính$2s/3$. Điều này cho thấy việc phân nhánh làm giảm đường kính hiệu dụng. 

Nếu một phương pháp giả định không chính xác rằng cấu trúc tốt nhất luôn là một đường dẫn hoặc luôn là một phân phối giống như ngôi sao, thì nó sẽ thất bại trong trường hợp ngược lại. Giải pháp đúng phải phù hợp với số lượng lá. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu bằng cách tưởng tượng chúng ta đoán tất cả các trọng số của cạnh tuân theo ràng buộc tổng và tính đường kính bằng cách chạy DFS giữa tất cả các cặp đỉnh. Ngay cả khi chúng ta chỉ cố gắng tối ưu hóa trọng số theo nghĩa liên tục, chúng ta vẫn cần phải suy luận về vô số phép gán, và thậm chí việc rời rạc hóa sẽ bùng nổ về mặt tổ hợp. Bất kỳ nỗ lực nào để mô phỏng phân phối ứng cử viên ít nhất đều dẫn đến độ phức tạp theo cấp số nhân về số cạnh. 

Điều quan trọng cần lưu ý là chỉ có số lượng lá mới quan trọng chứ không phải hình dạng chính xác của các nhánh bên trong. Một khi cây có$k$các lá, bất kỳ đường đi nào tạo nên đường kính đều có thể được coi là đi từ lá này sang lá khác thông qua một số cấu trúc bên trong. Chiến lược tối ưu cố gắng “đẩy” trọng lượng đều về phía các lá, để không có cặp lá nào cách nhau quá xa. 

Ý tưởng cấu trúc cơ bản là định hướng cây xung quanh một điểm trung tâm sao cho tất cả các lá được kết nối thông qua một trung tâm cân bằng. Sau đó, con đường xấu nhất được xác định bằng việc có bao nhiêu cành lá buộc phải chia sẻ tổng trọng lượng. Nếu có nhiều lá thì trọng lượng phải chia nhỏ hơn, giảm khoảng cách tối đa theo cặp. Nếu chỉ có hai lá (một đường dẫn), việc phân chia như vậy sẽ không giúp ích được gì. 

Điều này dẫn đến một đặc tính đơn giản: nếu có$k$lá, đường kính tối ưu tỷ lệ thuận với$2s/k$. Theo trực giác, mỗi chiếc lá đóng góp một “phần ngân sách” xấp xỉ$s/k$và đường đi dài nhất giữa hai lá sử dụng hai đóng góp như vậy. 

Điều này thu gọn vấn đề từ tối ưu hóa toàn cầu đối với các số liệu cây thành một vấn đề đếm đơn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trọng lượng Brute Force | hàm mũ | cao | Quá chậm | 
| Công thức đếm lá |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lá trên cây. Một chiếc lá là một đỉnh có bậc 1. Bước này nắm bắt số lượng điểm cuối độc lập phải “chia sẻ” tổng trọng số. 
2. Nếu cây có đúng hai lá thì trả về$s$. Điều này tương ứng với biểu đồ đường dẫn, trong đó tất cả trọng lượng nằm trên một chuỗi và không thể phân phối lại giữa các nhánh. 
3. Ngược lại hãy tính$k$, số lượng lá 
4. Tính đáp án dưới dạng$\frac{2s}{k}$. Điều này thể hiện khoảng cách trong trường hợp xấu nhất giữa hai lá khi tổng trọng lượng được phân bố đều trên tất cả các cành lá. 
5. Xuất giá trị này dưới dạng số dấu phẩy động. 

Lý do đằng sau bước 4 là mọi đường dẫn từ lá này sang lá khác đều phải đi qua lõi bên trong của cây. Nếu mỗi lá được gán một phần bằng nhau trong tổng trọng lượng thì khoảng cách từ lá đến tâm là hiệu quả.$s/k$và đường kính bao gồm hai đoạn như vậy. 

### Tại sao nó hoạt động 

Bất kỳ cây nào cũng có thể bị phân hủy thành lõi nơi các đỉnh bên trong phân phối dòng chảy giữa các lá. Vì mỗi chiếc lá đều góp phần
