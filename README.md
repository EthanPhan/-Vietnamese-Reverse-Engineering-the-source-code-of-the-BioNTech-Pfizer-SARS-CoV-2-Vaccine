# Giải mã mã nguồn của vắc-xin SARS-CoV-2 từ BioNTech/Pfizer


Xin chào! Trong bài này chúng ta sẽ phân tích chi tiết toàn bộ mã nguồn của vắc-xin mRNA SARS-CoV-2 tới từ BioNTech/Pfizer.

> Mình không phải tác giả mà dịch từ bản gốc tiếng anh [ở đây](https://berthub.eu/articles/posts/reverse-engineering-source-code-of-the-biontech-pfizer-vaccine/). Background của mình không phải là biotech nên trong quá trình dịch không thể tránh khỏi sai sót. Mọi đóng góp xin liên hệ với mình qua facebook [Ethan Phan](https://www.facebook.com/ethan.phan.79230/)

Điều này có thể nghe hơi lạ, vắc-xin là thứ chất lỏng để tiêm vào người thì tại sao chúng ta lại đan nói về mã nguồn ở đây?

Đây là một câu hỏi rất hay, hãy bắt đầu bằng việc nhìn vào một phần nhỏ của mã nguồn của vắc-xin từ BioNTech/Pfizer, được biết tới dưới cái tên BNT162b2, các tên gọi khác bao gồm Tozinameran hay Comirnaty.

<p align="center">
<img width="700" src="https://berthub.eu/articles/bnt162b2.png" alt="500 ký tư đầu tiên trong mã nguồn của BNT162b2">
</p>
<p align="center"> 500 ký tự đầu tiền trong mã nguồn của vắc-xin BNT162b2. Nguồn: <a href="https://mednet-communities.net/inn/db/media/docs/11889.doc">World Health Organization</a> </p>

Vắc-xin mRNA BNT162b có thành phần quan trọng nhất là một bộ mã nguồn kỹ thuật số dài 4284 ký tự, khá ngắn gọn để nằm vừa trong vài facebook post. Trong giai đoạn đầu tiền của quá trình sản xuất vắc-xin, đoạn mã nguồn này sẽ được đưa vào một máy in DNA (vâng, bạn không nghe nhầm đâu, máy in DNA là có thật đó), thứ sẽ dịch đoạn mã này thành những phân tử DNA thực sự.
<p align="center">
<img width="400" src="https://berthub.eu/articles/bioxp-3200.jpg" alt="Máy in DNA BioXp 3200">
</p>
<p align="center"> Máy in DNA BioXp 3200 đến từ <a href="https://codexdna.com/products/bioxp-system/">Codex DNA</a> </p>

Máy in sẽ tạo ra một lượng DNA rất nhỏ, thứ mà sau nhiều bước xử lý sinh học và hoá học sẽ biến thành RNA (sẽ nói thêm về RNA sau) trong các lọ vắc-xin. Một liều 30 microgram hoá ra chứa đúng 30 microgram RNA. Ngoài ra còn có một hệ thống đóng gói chất béo rất thông minh để đưa mRNA vào trong tế bào người.

RNA là phiên bản "bộ nhớ làm việc" dễ bay hơi (volatile ‘working memory’) của DNA. DNA giống như là chiếc ổ cứng sinh học vậy. DNA rất bền bỉ, dung lượng lớn đồng thời cũng rất tin cậy. Nhưng cũng giống như những chiếc máy tính không thể thực thi lệnh từ ổ cứng, trước khi được thực thi, lệnh phải được copy vào một bộ nhớ nhanh hơn, mềm dẻo hơn nhưng cũng mong manh hơn nhiều.

Trong máy tính, đó là RAM, trong sinh học là RNA. Chúng tương đồng một cách đáng kinh ngạc. Không giống như ổ cứng, dữ liệu trong RAM rất dễ biến mất nếu không được "chăm sóc" thường xuyên. Lý do mà vắc-xin mRNA của Pfizer/BioNTech phải được bảo quản ở nhiệt độ rất thấp cũng vậy: RNA là một đoá hoa mỏng manh.

Mỗi ký tự trong RNA nặng khoảng 0.53·10⁻²¹ gram, nghĩa là có khoảng 6·10¹⁶ ký tự trong một liều vắc-xin 30 microgram. Tính theo bytes thì là khoảng 25 petabytes, tuy nhiên nó vẫn chỉ là đoạn mã 4284 ký tự được copy tầm 2000 tỷ lần. Do đó lượng thông tin thực sự của vắc-xin chỉ khoảng hơn 1 kilobyte mà thôi. Bản thân vi rút SARS-CoV-2 chứa khoảng 7.5 kilobyte thông tin.

## Một chút thông tin thêm về DNA và RNA

DNA mà một đoạn mã kỹ thuật số. Không giống như trong những chiếc máy tính dùng 0 và 1, DNA dùng các ký tự A, C, G và U/T (các ‘nucleotides’, ‘nucleosides’ hay ‘bases).

A, C, G và U/T là các phân tử được lưu trữ dưới dạng chuỗi trong DNA (hoặc RNA).

Trong máy tính, mỗi nhóm 8 bít được nhóm lại thành 1 byte, và dữ liệu thường được xử lý theo từng byte.

Tự nhiên lại nhóm 3 nucleotides thành 1 condon (mình không biết tiếng việt cho mấy cái này), và condon thường là đơn vị xử lý cơ bản. Một condon chứa 6 bit dữ liệu (2 bit trên mỗi ký tự, 3 ký tự một condon => 6 bit. Đồng nghĩa với việc một codon có thể có có 2⁶ = 64 giá trị khác nhau).

Khá là kỹ thuật số phải không? Nếu còn thắc mắc về vấn đề này bạn có thể tra khảo thêm thông tin từ [tài liệu của WHO](https://mednet-communities.net/inn/db/media/docs/11889.doc).
> Một số thông tin chi tiết thêm có thể được tìm thấy [ở đây](https://berthub.eu/articles/posts/what-is-life/) - Đường link này ('Cuộc sống là gì') có thể giúp hiểu hơn về phần còn lại của bài này. Nếu bạn thích video, đây là [video dài 2 tiếng từ tác giả](https://berthub.eu/dna).

## Vậy cái đoạn mã đó nó làm gì?

Ý tưởng chung của vắc-xin là dạy cho hệ miễn dịch cách để chống lại các mầm bệnh mà không cần phải nhiễm các mầm bệnh đó. Việc này từ trước cho tới nay được thực hiện bằng cách tiêm vào trong cơ thể vi rút đã được làm yếu hoặc vô hiệu hoá (bất hoạt), cộng với một chất bổ trợ để kích thích hệ miễn dịch phản ứng. Cách này cần tới hàng tỷ quả trứng (hoặc côn trùng) để sản xuất ra vắc-xin. Cách này cũng tốn rất nhiều thời gian và đoi hỏi nhiều sự may mắn. Đôi khi cả một chủng vi rút không liên quan gì tới căn bệnh cũng được sử dụng.

Vắc-xin mRNA đạt được mục tiêu tương tự ('dạy cho hệ miễn dịch cách chống lại mầm bệnh') nhưng theo một cách như là tia laser vậy: rất tập trung nhưng cũng rất mạnh mẽ.

Và đây là cách nó hoạt động. Liều vắc-xin mà chúng ta đang nói tới chứa mRNA (hay messenger RNA) miêu tả phân tử protein 'Gai' (phân tử protein này nhìn như cái gai nằm trên mặt tế bào vi rút vậy) của vi rút SARS-CoV-2. Thông qua các phương tiện hoá học, vắc-xin sẽ đưa mRNA này vào trong tế bào người.

Điều này sau đó sẽ kích hoạt quá trình sản xuất protein 'Gai' của SARS-CoV-2 bên trong cơ thể người với một số đủ lớn để kích hoạt hệ thống miễn dịch phản ứng. Với sự xuất hiện của các protein Gai trong cơ thể cùng các dấu hiệu giống với việc các tế bào cơ thể bị nhiễm bệnh, hệ thống miễn dịch của chúng ta sẽ phát triển một cơ chế phòng vệ mạnh mẽ để vô hiệu hoá các protein Gai và đồng thời chống lại việc sản xuất các protein này.

Và đó là thứ giúp chúng ta đạt được thứ vắc-xin với hiệu quả miễn dịch 95%.

## Mã nguồn!

Hãy bắt đầu từ đầu, nơi rất tốt để bắt đầu. Tài liệu của WHO có hình ảnh rất trực quan như thế này:
<p align="center">
<img width="500" src="https://berthub.eu/articles/vaccine-toc.png" alt="Schematic">
</p>

Đây cũng giống như một cái mục lục vậy. Chúng ta sẽ bắt đầu với cái 'cap' (mũ), được miêu tả trong hình bằng cái mũ.

Cũng giống như việc bạn không thể quẳng opcodes trong một file đi và chaỵ nó vậy, hệ điều hành sinh học cũng cần headers, linkers và calling conventions.

Mã nguồn của vắc-xin đang nhắc tới bắt đầu với hai ký tự sau đây:
```
GA
```

Đây có thể so sánh với việc mọi file chạy trên Windows và DOS bắt đầu với `MZ`, hay mọi UNIX script đều bắt đầu với `#!` vậy. Trong cả hệ điều hành máy tính lẫn sinh học, các ký tự này đều không được thực thi. Nhưng chúng vẫn cần phải có mặt ở đó vì thiếu chúng thì sẽ không có gì xảy ra cả.

Cái 'mũ' mRNA [có một số chức năng nhất định](https://en.wikipedia.org/wiki/Five-prime_cap#Function). Đầu tiên, nó đánh dấu rằng đoạn mã đến từ nhân tế bào. Tuy nhiên trong trường hợp này thì đoạn mã không tới từ nhân tế bào mà tới từ vắc-xin. Nhưng chúng ta không cần phải nói với tế bào việc đó. Cái cap khiến cho đoạn mã nhìn giống như những đoạn mã khác trong cơ thể và tránh cho nó bị phá huỷ.

Hai phân tử nucleotide `GA` đầu tiên này cũng hơi khác phần còn lại của RNA về mặt hoá học. Về mặt này thì `GA` có thể được coi như một báo hiệu ngoài dải.

## Vùng không dịch năm phẩy ("5′ untranslated region" hay 5′ UTR)

Các phân tử RNA chỉ có thể được đọc theo một chiều. Cái phần mà việc đọc bắt đầu được gọi là 5' hay "five-prime" (trong tiếng việt có thể đọc là "năm-phẩy"). Việc đọc kết thúc ở đoạn gọi là 3' hay ba-phẩy. Việc đặt tên này khá là khó hiểu.

Sự sống bao gồm các protein (hoặc những thứ tạo nên bởi protein). Và các protein này được miêu tả trong RNA. Quá trình chuyển RNA thành các protein được gọi là dịch (translation).

Ở đây chúng ta có vùng không dịch 5' (vì nó không được dịch nên gọi là vùng không dịch), vậy nên đoạn mã này sẽ không xuất hiện trong phân tử protein:
```
GAAΨAAACΨAGΨAΨΨCΨΨCΨGGΨCCCCACAGACΨCAGAGAGAACCCGCCACC
```

Ở đây chúng ta có một sự ngạc nhiên đầu tiên. Bình thường RNA bao gồm các ký tự A, C, G và U. Ký tự U trong DNA được gọi là T. Nhưng ở đây lại có ký tự Ψ (đọc là 'sai'). Chuyện gì đang xảy ra vậy?

Đây là một phần rất thông minh về vắc-xin chúng ta đang nói tới. Cơ thể chúng ta có một hệ thống diệt vi rút rất mạnh mẽ. Vì lý do này các tế bào cực kỳ không hứng thú với RNA ngoại lai và rất cố gắng để phá huỷ nó trước khi nó kịp làm bất cứ điều gì.

Đây là một vấn đề với vắc-xin của chúng ta - nó cần vượt qua hệ miễn dịch. Qua nhiều năm nghiên cứu chúng ta đã phát hiện ra rằng nếu thay thế các phân tử U trong RNA bằng một phiên bản hơi khác của nó, hệ miễn dịch của chúng ta sẽ bỏ qua nó. Thật đấy.

Vậy nên trong vắc-xin của BioNTech/Pfizer, tất cả phân tử U được thay thế bằng phân tử 1-methyl-3’-pseudouridylyl, được ký hiệu bởi Ψ. Phần hết sức thông minh về việc này là ở chỗ mặc dù phân tử Ψ thay thế giúp RNA vượt qua hệ miễn dịch, nó vẫn được chấp nhận như phân tử U bình thường trong tế bào.

Trong bảo mật máy tính có một chiêu tương tự - đôi khi một gói tin đã bị làm méo có chủ đích có thể được gửi để đánh lừa firewall và các phần mềm bảo mật, nhưng nó vẫn được chấp nhận ở backend servers - thứ mà có thể bị hack bằng gói tin kia.

Chúng ta giờ đây đang gặt hái thành quả của các nghiên cứu khoa học nền tảng được thực hiện trong quá khứ. [Những nhà nghiên cứu](https://twitter.com/PennMedicine/status/1341766354232365059) khám phá ra kỹ thuật thay thế U bằng Ψ đã phải đấu tranh để giành được tiền tài trợ cho [nghiên cứu của họ](https://www.statnews.com/2020/11/10/the-story-of-mrna-how-a-once-dismissed-idea-became-a-leading-technology-in-the-covid-vaccine-race/). Tất cả chúng ta nên biết ơn vì điều đó, và tôi tin rằng [sớm thôi giải Nobel sẽ về tới tay họ](https://twitter.com/PowerDNS_Bert/status/1329861047168225281).

> Nhiều người hỏi rằng liệu vi rút có dùng kỹ thuật thay thế U bằng Ψ để vượt qua hệ miễn dịch không? Ngắn gọn là cực kỳ khó xảy ra. Sự sống đơn giản là không có phương tiện cần thiết để tạo ra phân tử 1-methyl-3’-pseudouridylyl. Các vắc-xin mRNA sẽ nhanh chóng bị tiêu biến trong cơ thể con người, và chẳng có cơ hội nào để các phân tử RNA với ký tự Ψ tồn tại trong đó cả. ["No, Really, mRNA Vaccines Are Not Going To Affect Your DNA"](https://www.deplatformdisease.com/blog/no-really-mrna-vaccines-are-not-going-to-affect-your-dna) là một bài rất đáng để đọc về vấn đề này.
