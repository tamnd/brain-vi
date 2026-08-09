---
title: "CF 104011B - Boris và Berta"
description: "Bài tập 225 xây dựng một ZDD có đường đi mã hóa tất cả các đường đi đơn giản giữa hai đỉnh cố định $s$ và $t$. Việc xây dựng tiến hành bằng cách tìm kiếm có kiểm soát trên các tập cạnh một phần: mỗi nút ZDD biểu thị trạng thái của đường dẫn một phần và mỗi quyết định tương ứng với việc bao gồm hoặc…"
date: "2026-07-02T05:13:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104011
codeforces_index: "B"
codeforces_contest_name: "2021-2022 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104011
solve_time_s: 128
verified: false
draft: false
---

[CF 104011B – Boris và Berta](https://codeforces.com/problemset/problem/104011/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
##Giải pháp 

Bài tập 225 xây dựng một ZDD có đường đi mã hóa tất cả các đường đi đơn giản giữa hai đỉnh cố định$s$Và$t$. Việc xây dựng tiến hành bằng cách tìm kiếm có kiểm soát trên các tập cạnh một phần: mỗi nút ZDD biểu thị trạng thái của đường dẫn một phần và mỗi quyết định tương ứng với việc bao gồm hoặc loại trừ một cạnh ứng cử viên, trong khi duy trì bất biến rằng các cạnh được chọn tạo thành một đường dẫn đơn giản từ điểm cuối hiện tại đến đích mà không lặp lại các đỉnh. 

Để có được ZDD cho tất cả các chu trình đơn giản, thay đổi cơ bản là chu trình không có điểm cuối phân biệt, trong khi đường dẫn thì có. Khó khăn là việc quy giản một cách ngây thơ thành “các đường dẫn từ$v$quay lại$v$" vượt qua mỗi chu kỳ nhiều lần, một lần cho mọi điểm và hướng bắt đầu có thể. Do đó, việc xây dựng phải áp đặt một biểu diễn chuẩn của từng chu kỳ trong khi vẫn phù hợp với cùng một khung chuyển đổi trạng thái được sử dụng cho các đường dẫn. 

Ý tưởng chính là neo mọi chu trình ở đỉnh được gắn nhãn tối thiểu của nó. Điều này loại bỏ tính đối xứng quay và làm cho mỗi chu trình tương ứng với chính xác một biểu diễn gốc. ZDD sau đó được xây dựng như một sự kết hợp trên tất cả các điểm neo có thể có, trong đó mỗi ZDD con được neo sẽ liệt kê các chu trình bắt đầu và kết thúc ở cùng một đỉnh nhưng bị hạn chế sao cho không có đỉnh nhỏ hơn nào được ghé thăm. 

Sửa thứ tự các đỉnh phù hợp với thứ tự biến BDD được sử dụng trong bài tập 225. Đối với mỗi đỉnh$s$, xác định ZDD đại diện cho tất cả các chu trình đơn giản$C$như vậy$\min(C)=s$. ZDD cuối cùng là sự kết hợp trên tất cả$s$của các cấu trúc này. 

Bên trong công trình cố định$s$, chúng ta sửa đổi đường dẫn ZDD từ bài tập 225 bằng cách thay thế điều kiện cuối “tầm$t$” với “trở lại$s$”, nhưng với một ràng buộc nghiêm ngặt ngăn cản việc quay lại$s$trước bước cuối cùng. Tất cả các đỉnh trung gian trong đường một phần bắt buộc phải lớn hơn$s$. Điều này đảm bảo rằng$s$là đỉnh tối thiểu duy nhất của mỗi chu kỳ được tạo ra. 

Trạng thái được duy trì bởi đệ quy giống như trong quá trình xây dựng đường dẫn: điểm cuối hiện tại$v$cùng với tập hợp các đỉnh ngầm định đã được sử dụng, được biểu thị bằng lịch sử đường dẫn ZDD. Chuyển tiếp tương ứng với việc chọn một cạnh không sử dụng$(v,u)$, điều này chỉ được phép nếu$u$chưa xuất hiện sớm hơn trong đường dẫn và$u > s$. Sự hạn chế$u > s$thực thi điều kiện tối thiểu chính tắc, trong khi ràng buộc đơn giản đảm bảo không lặp lại đỉnh. 

Một chu kỳ chỉ được hoàn thành khi điểm cuối hiện tại$v$liền kề với$s$và đường một phần chứa ít nhất một cạnh. Vào lúc đó, chiếm ưu thế$(v,s)$được phép như một sự chuyển tiếp cuối cùng sang thiết bị đầu cuối$1$nút của ZDD. Không được phép chuyển tiếp sớm hơn$s$, do đó bước đóng là duy nhất và xảy ra đúng một lần trong mỗi chu kỳ. 

Về mặt hình thức, đối với mỗi$s$, đệ quy khám phá tất cả các tập hợp con cạnh$E' \subseteq E$thỏa mãn các điều kiện sau: đồ thị con$(V,E')$là một chu trình đơn giản,$s \in V(E')$, và mọi$v \in V(E') \setminus {s}$thỏa mãn$v > s$. Cấu trúc ZDD thực thi các điều kiện này cục bộ tại mỗi nút quyết định bằng cách lọc các chuyển tiếp được chấp nhận. 

## Tại sao nó hoạt động 

Mỗi chu trình đơn giản có một đỉnh duy nhất có chỉ số tối thiểu theo thứ tự cố định. Đặt đỉnh đó là$s$. Bắt đầu từ$s$, việc đi qua chu trình theo một trong hai hướng sẽ tạo ra một đường đi đơn giản không bao giờ đi qua một đỉnh nhỏ hơn$s$trước khi quay lại$s$. Do đó, chu trình xuất hiện chính xác ở một trong các ZDD neo được xây dựng cho$s$. 

Ngược lại, bất kỳ tính toán chấp nhận nào trong ZDD neo cho$s$tạo ra một cuộc đi bộ khép kín bắt đầu tại$s$, không bao giờ quay lại các đỉnh, không bao giờ rời khỏi tập hợp các đỉnh lớn hơn$s$, và quay trở lại$s$đúng một lần khi kết thúc. Những điều kiện này buộc tập cạnh kết quả là một chu trình đơn giản với đỉnh tối thiểu$s$. 

Do đó, mỗi chu trình đơn giản tương ứng với chính xác một đường dẫn chấp nhận trong đúng một ZDD được neo và mọi đường dẫn chấp nhận tương ứng với chính xác một chu trình đơn giản. Điều này thiết lập sự song ánh giữa các chu trình trong biểu đồ và các giải pháp được biểu thị bằng cấu trúc ZDD cuối cùng. Điều này hoàn thành việc chứng minh. ∎
