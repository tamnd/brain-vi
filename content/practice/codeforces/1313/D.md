---
title: "CF 1313D - Chúc mừng năm mới"
description: "Chúng ta được cung cấp một dòng lớn các vị trí từ 1 đến $m$ và một tập hợp các khoảng $n$. Mỗi khoảng thời gian đại diện cho một “câu thần chú” mà khi sử dụng sẽ thêm một viên kẹo vào mọi vị trí trong phạm vi của nó. Mỗi phép thuật có thể được sử dụng nhiều nhất một lần."
date: "2026-06-11T17:10:25+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1313
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 622 (Div. 2)"
rating: 2500
weight: 1313
solve_time_s: 153
verified: false
draft: false
---

[CF 1313D - Chúc mừng năm mới](https://codeforces.com/problemset/problem/1313/D) 

**Đánh giá:** 2500 
**Thẻ:** bitmask, dp, triển khai 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng lớn các vị trí từ 1 đến$m$, và một bộ sưu tập$n$khoảng thời gian. Mỗi khoảng thời gian đại diện cho một “câu thần chú” mà khi sử dụng sẽ thêm một viên kẹo vào mọi vị trí trong phạm vi của nó. Mỗi phép thuật có thể được sử dụng nhiều nhất một lần. 

Sau khi chọn một tập hợp con các phép thuật, mỗi vị trí sẽ tích lũy được một số lượng kẹo nhất định bằng với số khoảng thời gian đã chọn bao phủ nó. Một vị trí được coi là “vui vẻ” nếu số này là số lẻ và “không vui” nếu ngược lại. Mục tiêu là chọn một tập hợp con các khoảng tối đa hóa số lượng vị trí có số lượng bao phủ lẻ. 

Giải thích trực tiếp là mỗi khoảng chuyển đổi tính chẵn lẻ của tất cả các vị trí trong phạm vi của nó. Chúng tôi đang cố gắng tối đa hóa số điểm đạt được mức chẵn lẻ 1 sau khi áp dụng một tập hợp con của các lần lật phạm vi này. 

Khó khăn chính đến từ những hạn chế. Số lượng khoảng thời gian$n$tùy thuộc vào$10^5$, trong khi phạm vi tọa độ$m$tùy thuộc vào$10^9$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào xây dựng một mảng có kích thước một cách rõ ràng$m$hoặc mô phỏng từng vị trí. Bất kỳ cách tiếp cận hợp lệ nào cũng phải nén cấu trúc của vấn đề vào các điểm cuối của các khoảng, vì chỉ những điều đó mới quan trọng đối với những thay đổi trong phạm vi bao phủ. 

Ràng buộc thứ hai là mỗi vị trí có nhiều nhất$k \le 8$kẹo nếu tất cả các phép thuật được sử dụng. Điều này rất quan trọng vì nó giới hạn cấu trúc chồng chéo cục bộ và cho phép các ý tưởng nén trạng thái. 

Một vấn đề tế nhị là các vị trí độc lập ngoại trừ sự trùng lặp về khoảng thời gian. Tuy nhiên, các khoảng tương tác theo cách tổng thể: việc chọn một khoảng sẽ thay đổi tính chẵn lẻ trên toàn bộ phân đoạn, điều này ảnh hưởng đến tất cả các khoảng khác chồng lên nó. 

Một sai lầm ngây thơ nhưng đầy cám dỗ là nghĩ đến việc lựa chọn tham lam các khoảng thời gian với mức “lợi ích” lớn nhất. Ví dụ: hãy xem xét các khoảng:```
1 5
2 6
3 7
```Bất kỳ nỗ lực tham lam nào để chọn các khoảng có lợi cục bộ đều thất bại vì hiệu ứng tương đương tương tác và một khoảng tốt cục bộ có thể hủy bỏ lợi ích trong tương lai. 

Một trường hợp cạnh khác là khi các khoảng giống hệt nhau hoặc chồng chéo nhiều. Nếu hai khoảng giống hệt nhau được chọn, chúng sẽ hủy bỏ tính chẵn lẻ, do đó việc chọn cả hai tương đương với việc không chọn gì. Một cách tiếp cận đơn giản xử lý từng khoảng một cách độc lập sẽ bỏ sót cấu trúc XOR này. 

## Phương pháp tiếp cận 

Nếu bỏ qua các ràng buộc, chúng ta có thể thử lặp qua tất cả các tập hợp con của các khoảng. Đối với mỗi tập hợp con, chúng tôi mô phỏng phạm vi bao phủ trên tất cả các vị trí và đếm xem có bao nhiêu vị trí được bao phủ với số lần lẻ. Điều này đúng vì nó phù hợp trực tiếp với định nghĩa về hạnh phúc. Tuy nhiên, điều này đòi hỏi$2^n$tập hợp con và cho$n = 10^5$, điều này hoàn toàn không thể thực hiện được. Ngay cả đối với$n = 40$, điều này trở thành đường biên giới. 

Một cách mạnh mẽ hơn một chút là nén tọa độ và mô phỏng vùng phủ sóng bằng cách sử dụng tổng tiền tố cho mỗi tập hợp con. Điều này vẫn giữ nguyên sự lựa chọn theo cấp số nhân của các tập hợp con, do đó nút cổ chai vẫn còn. 

Quan sát cấu trúc quan trọng là câu trả lời chỉ phụ thuộc vào tính chẵn lẻ của phạm vi bao phủ và mỗi khoảng đóng góp một sự chuyển đổi theo bit trên một phân đoạn. Đây là cấu trúc “XOR của các khoảng” cổ điển. 

Nếu chúng ta quét đường này, trạng thái ở bất kỳ vị trí nào chỉ phụ thuộc vào khoảng hoạt động nào hiện đang đóng góp. Khi chúng ta di chuyển từ trái sang phải, các khoảng sẽ vào và ra khỏi tập hoạt động. Từ$k \le 8$, tối đa 8 khoảng có thể trùng nhau tại bất kỳ điểm nào theo nghĩa cấu hình tối ưu, điều này cho thấy rằng hệ thống có kích thước hiệu quả nhỏ ở bất kỳ sự kiện ranh giới nào. 

Chúng tôi sắp xếp các điểm cuối trong khoảng thời gian và xử lý các sự kiện. Giữa các điểm cuối liên tiếp, tập hợp các khoảng hoạt động được cố định. Điều quan trọng là tính chẵn lẻ của số lượng khoảng được chọn bao gồm phân khúc đó. Vì vậy, vấn đề trở thành: đối với mỗi phân đoạn giữa các tọa độ nén liên tiếp, hãy quyết định chọn tập hợp con nào của các khoảng hoạt động và đảm bảo tính nhất quán khi các khoảng bắt đầu và kết thúc. 

Điều này dẫn đến trạng thái DP trên đường quét trong đó trạng thái là tập hợp con hiện tại của các khoảng hoạt động. Vì tại bất kỳ thời điểm nào, số khoảng thời gian hoạt động nhiều nhất là$k \le 8$, số lượng trạng thái được giới hạn bởi$2^k \le 256$. Quá trình chuyển đổi tương ứng với việc thêm hoặc xóa các khoảng thời gian ở điểm cuối, đồng thời tùy ý chọn xem mỗi khoảng thời gian có được đưa vào tập hợp con cuối cùng hay không. 

Chúng tôi tối đa hóa tổng đóng góp trên các phân khúc mà trạng thái tạo ra mức tương đương nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^n \cdot m)$|$O(m)$| Quá chậm | 
| Quét dòng + DP trên các tập hợp con đang hoạt động |$O(n \cdot 2^k)$|$O(2^k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành quét qua các điểm cuối khoảng được sắp xếp. 

1. Chúng tôi nén tất cả các điểm cuối trong khoảng vào một danh sách được sắp xếp các vị trí quan trọng. Giữa các tọa độ liên tiếp, không có gì thay đổi, vì vậy mỗi phân đoạn sẽ đóng góp độc lập sau khi chúng tôi ấn định khoảng thời gian nào đang hoạt động. 
2. Chúng tôi sắp xếp các sự kiện theo vị trí, đánh dấu thời điểm bắt đầu và kết thúc các khoảng thời gian. Chúng tôi quét từ trái sang phải, duy trì tập hợp các khoảng thời gian hoạt động hiện tại. Mỗi khoảng hoạt động tương ứng với một lựa chọn nhị phân: được chọn hoặc không được chọn trong tập hợp con cuối cùng. 
3. Tại mỗi phân đoạn, chúng tôi xem xét tất cả các tập hợp con của các khoảng thời gian hiện đang hoạt động. Đối với một tập hợp con cố định, sự đóng góp chẵn lẻ trên phân đoạn này được xác định bởi số lượng khoảng được chọn đang hoạt động trên nó. Nếu số này là số lẻ thì mọi vị trí trong đoạn đều trở nên vui vẻ, vì vậy chúng tôi thêm độ dài đoạn vào điểm. 
4. Chúng tôi duy trì một mảng DP trên các tập hợp con của các khoảng thời gian hoạt động. Khi chúng tôi di chuyển từ vị trí sự kiện này sang vị trí sự kiện tiếp theo, chúng tôi chuyển trạng thái DP bằng cách cập nhật khoảng thời gian nào vào hoặc rời khỏi tập hoạt động. Loại bỏ trạng thái thu gọn khoảng thời gian bằng cách loại bỏ bit đó; việc thêm giới thiệu một chiều mới và mở rộng trạng thái bằng cách sao chép các giá trị hiện có qua các lựa chọn của bit mới. 
5. Ở mỗi bước, chúng tôi cập nhật DP với số điểm tốt nhất có thể đạt được cho từng cấu hình tập hợp con, tích lũy đóng góp của phân khúc hiện tại. 

Sau khi xử lý tất cả các phân đoạn, giá trị tối đa trên tất cả các trạng thái DP là câu trả lời. 

### Tại sao nó hoạt động 

Trạng thái DP mã hóa chính xác khoảng thời gian hoạt động nào được chọn trong giải pháp cuối cùng tại một vị trí quét nhất định. Bất kỳ lựa chọn toàn cục hợp lệ nào đều tương ứng với chính xác một đường đi qua các trạng thái này khi các khoảng được đưa vào và loại bỏ. Sự đóng góp của mỗi phân đoạn chỉ phụ thuộc vào tính chẵn lẻ của các khoảng được chọn bao trùm phân đoạn đó, được xác định đầy đủ bởi trạng thái hiện tại. Vì quá trình chuyển đổi duy trì tính nhất quán của các quyết định đưa vào trong quá trình quét nên không có cấu hình hợp lệ nào bị bỏ sót và không có cấu hình không hợp lệ nào được đưa ra. 

## Giải pháp Python```
PythonRun
```
