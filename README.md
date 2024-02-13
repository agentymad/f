const Discord = require('discord.js');


module.exports = {
  data: new Discord.SlashCommandBuilder()
  .setName('mülk')
  .setDescription('mülk')

 //categories: new categories = interaction.client.channels.cache.filter(channel => channel.type === 'GUILD_CATEGORY');

  .addSubcommandGroup(group => group.setName('market').setDescription('market')
  .addSubcommand(subcommand => subcommand.setName('ekle').setDescription('Markete mülk eklersiniz.')
  .addStringOption(option => option.setName('emoji').setDescription('Mülkün emojisini girin.').setRequired(true))
  .addStringOption(option => option.setName('isim').setDescription('Mülkün ismini girin.').setRequired(true))
  .addStringOption(option => option.setName('fiyat').setDescription('Mülkün fiyatını girin.').setRequired(true))
  .addStringOption(option => option.setName('tip').setDescription('Mülkün tipini girin. (Araç, Ev, Eşya)').setRequired(true))
  .addRoleOption(option => option.setName('rol').setDescription('Mülkün rolünü seçin.').setRequired(true))
  .addRoleOption(option => option.setName('zorunlu-rol').setDescription('Mülkün zorunlu rolünü seçin. (Mülkü almak için bu role sahip olmak zorunda)'))
  .addBooleanOption(option => option.setName('tek-seferlik').setDescription('Mülkü bir kez alınabilir olsun mu? (Mülk rolüne sahip olunca bir daha alamaz)'))
  .addChannelOption(option => option.setName('kategori').setDescription('Mülkün kategorisini seçin.').setRequired(false)))

  .addSubcommand(subcommand => subcommand.setName('sil').setDescription('Marketteki mülkü silersiniz.')
  .addStringOption(option => option.setName('mülk').setDescription('Silmek istediğiniz mülkü seçin.').setRequired(true).setAutocomplete(true)))

 .addSubcommand(subcommand => subcommand.setName('düzenle').setDescription('Marketteki mülkü düzenlersiniz.')
  .addStringOption(option => option.setName('mülk').setDescription('Düzenlemek istediğiniz mülkü seçin.').setRequired(true).setAutocomplete(true))
  .addStringOption(option => option.setName('emoji').setDescription('Mülkün emojisini girin.'))
  .addStringOption(option => option.setName('isim').setDescription('Mülkün ismini girin.'))
  .addStringOption(option => option.setName('fiyat').setDescription('Mülkün fiyatını girin.'))
  .addStringOption(option => option.setName('tip').setDescription('Mülkün tipini girin. (Araç, Ev, Eşya)'))
  .addRoleOption(option => option.setName('rol').setDescription('Mülkün rolünü seçin.'))
  .addRoleOption(option => option.setName('zorunlu-rol').setDescription('Mülkün zorunlu rolünü seçin. (Mülkü almak için bu role sahip olmak zorunda)'))
  .addBooleanOption(option => option.setName('tek-seferlik').setDescription('Mülkü bir kez alınabilir olsun mu? (Mülk rolüne sahip olunca bir daha alamaz)'))
  .addChannelOption(option => option.setName('kategori').setDescription('Mülkün kategorisini seçin.').setRequired(false)))








  .addSubcommand(subcommand => subcommand.setName('liste').setDescription('Marketteki mülkleri listeler.')))

  .addSubcommand(subcommand => subcommand.setName('görüntüle').setDescription('Seçilen kullanıcının mülklerini görüntüler.')
  .addUserOption(option => option.setName('kullanıcı').setDescription('Mülklerini görüntüleyeceğiniz kullanıcıyı seçin.')))

  .addSubcommand(subcommand => subcommand.setName('ver').setDescription('Seçilen kullanıcıya mülk verirsiniz.')
  .addUserOption(option => option.setName('kullanıcı').setDescription('Mülk verilecek kullanıcıyı seçin.').setRequired(true))
  .addStringOption(option => option.setName('mülk').setDescription('Verilecek mülkü seçin.').setRequired(true).setAutocomplete(true)))

  .addSubcommand(subcommand => subcommand.setName('ekle').setDescription('Seçilen kullanıcıya mülk eklersiniz.')
  .addUserOption(option => option.setName('kullanıcı').setDescription('Mülk eklemek istediğiniz kullanıcıyı seçin.').setRequired(true))
  .addStringOption(option => option.setName('mülk').setDescription('Eklemek istediğiniz mülkü seçin.').setRequired(true).setAutocomplete(true)))

  .addSubcommand(subcommand => subcommand.setName('sil').setDescription('Seçilen kullanıcıdan mülk silersiniz.')
  .addUserOption(option => option.setName('kullanıcı').setDescription('Mülk silmek istediğiniz kullanıcıyı seçin.').setRequired(true))
  .addStringOption(option => option.setName('mülk').setDescription('Silmek istediğiniz mülkü seçin.').setRequired(true).setAutocomplete(true)))
  
  .toJSON(),
  permissions: {
    /**
     * RoleId: 0,
     * UserId: 1,
     * RoleName: 2,
     * Permission: 3
     */
    'mülk market ekle': {
      3: ['Administrator']
    },
    'mülk market sil': {
      3: ['Administrator']
    },
    'mülk market düzenle': {
      3: ['Administrator']
    },
    'mülk market liste': {
      0: ['1133396409459150953']
    },
    'mülk görüntüle': {
      0: ['1133396409459150953']
    },
    'mülk ver': {
      0: ['1133396409459150953']
    },
    'mülk ekle': {
      3: ['Administrator']
    },
    'mülk sil': {
      3: ['Administrator']
    }
  },
  
  execute: async (interaction, { database, group, subcommand }) => {
    switch (group) {
      case 'market':
        switch (subcommand) {
case 'ekle':
  var emoji = interaction.options.getString('emoji');
  var name = interaction.options.getString('isim');
  var price = interaction.options.getString('fiyat');
  var type = interaction.options.getString('tip');
  var role = interaction.options.getRole('rol');
  var requiredRole = interaction.options.getRole('zorunluRol');
  var oneTimePurchase = interaction.options.getBoolean('tekSeferlik');
  var category = interaction.options.getChannel('kategori');
  if (!emoji) return interaction.error('Emoji bulunamadı.');
  if (!name) return interaction.error('İsim bulunamadı.');
  if (!price) return interaction.error('Fiyat bulunamadı.');
  if (!type) return interaction.error('Tip bulunamadı.');
  if (!role) return interaction.error('Rol bulunamadı.');

  await interaction.deferReply();

  // Kategori seçeneği varsa ve kategori ID'si varsa databse'e ekleyelim, yoksa null olarak ekleyelim.
  var categoryID = category ? category.id : null;

  var id = interaction.client.properties.functions.generateId();
  await database.set(`market.${id}`, { emoji, name, price: Number(price), type, role: role.id, requiredRole: requiredRole ? requiredRole.id : null, oneTimePurchase, categoryID });
  return interaction.success(`${emoji} **${name}** markete **${Number(price).toLocaleString('en-US')}₺** fiyatıyla eklendi.`);

          case 'sil':
            var id = interaction.options.getString('mülk');
            var market = database.get(`market`) || {};
            var data = market[id];

            if (!data) return interaction.error('Mülk bulunamadı.');

            await interaction.deferReply();
            await database.delete(`market.${id}`);
            return interaction.success(`${data.emoji} **${data.name}** marketten silindi.`);

          case 'düzenle':
            var id = interaction.options.getString('mülk');
            var emoji = interaction.options.getString('emoji');
            var name = interaction.options.getString('isim');
            var price = interaction.options.getString('fiyat');
            var type = interaction.options.getString('tip');
            var role = interaction.options.getRole('rol');
            var requiredRole = interaction.options.getRole('zorunluRol');
            var oneTimePurchase = interaction.options.getBoolean('tekSeferlik');
			var category = interaction.options.getChannel('kategori');
            var market = database.get('market') || {};
            var data = market[id];
var categoryID = category ? category.id : null;

            if (!data) return interaction.error('Mülk bulunamadı.');
            if (!emoji && !name && !price && !type && !role && !requiredRole && !oneTimePurchase && !category) return interaction.error('En az bir parametre düzenlemelisin.');

            await interaction.deferReply();
            if (emoji) await database.set(`market.${id}.emoji`, emoji);
            if (name) await database.set(`market.${id}.name`, name);
            if (price) await database.set(`market.${id}.price`, Number(price));
            if (type) await database.set(`market.${id}.type`, type);
            if (role) await database.set(`market.${id}.role`, role.id);
            if (requiredRole) await database.set(`market.${id}.requiredRole`, requiredRole.id);
            if (oneTimePurchase) await database.set(`market.${id}.oneTimePurchase`, oneTimePurchase);
if (category) await database.set(`market.${id}.categoryID`, categoryID);
            return interaction.success(`${data.emoji} **${data.name}** mülkü düzenlendi.`);

          case 'liste':
            var market = database.get('market') || {};
            if (Object.keys(market).length <= 0) return interaction.error('Hiç mülk bulunamadı.');
            var types = [...new Set(Object.values(market).map(value => value.type))];

            var embed = new Discord.EmbedBuilder()
            .setColor('#adadad')
            .setAuthor({ name: interaction.guild.name, iconURL: interaction.guild.iconURL() })

            var fields = [];
            for (const type of types) fields.push({ name: type, value: Object.entries(market).filter(([, data]) => data.type === type).map(([, data]) => `- ${data.emoji} **${data.name}** | **${data.price.toLocaleString('en-US')}₺**`).join('\n') });
            embed.setFields(fields);
            return interaction.reply({ embeds: [embed] });
        };
      
      case null:
        switch (subcommand) {
          case 'görüntüle':
            var member = interaction.options.getMember('kullanıcı') || interaction.member;
            var market = database.get('market') || {};
            var mülkler = (database.get(`mülkler.${member.user.id}`) || []).filter(value => market[value.dataId]);
            if (mülkler.length <= 0) return interaction.error('Hiç mülk bulunamadı.');

            var types = [...new Set(mülkler.map(value => market[value.dataId].type))];
            
            var embed = new Discord.EmbedBuilder()
            .setColor('#adadad')
            .setAuthor({ name: member.displayName, iconURL: member.user.displayAvatarURL() })
            .setFooter({ text: interaction.user.username, iconURL: interaction.user.displayAvatarURL() })
            .setTimestamp();

            var fields = [];
            for (const type of types) fields.push({ name: type, value: mülkler.filter(value => market[value.dataId].type === type).map(value => `- ${market[value.dataId].emoji} **${market[value.dataId].name}** (${value.generatedId})`).join('\n') });
            embed.setFields(fields);

            return interaction.reply({ embeds: [embed] });

          case 'ver':
            var member = interaction.options.getMember('kullanıcı');
            var id = interaction.options.getString('mülk');
            var market = database.get('market') || {};
            var mülkler = (database.get(`mülkler.${interaction.user.id}`) || []).filter(value => market[value.dataId]);
            var data = market[mülkler.find(value => value.generatedId === id).dataId];

            if (!member || member.user.id == interaction.user.id) return interaction.error('Kullanıcı bulunamadı.');
            if (!data) return interaction.error('Mülk bulunamadı.');

            await interaction.deferReply();
            await database.set(`mülkler.${member.user.id}`, (database.get(`mülkler.${member.user.id}`) || []).concat({ dataId: mülkler.find(value => value.generatedId === id).dataId, generatedId: id }));
            await database.set(`mülkler.${interaction.user.id}`, (database.get(`mülkler.${interaction.user.id}`) || []).filter(value => value.generatedId !== id));
            await member.roles.add(data.role).catch(() => null);
            return interaction.success(`${data.emoji} **${data.name}** isimli mülkü **${member.user.username}** kullanıcısına verdiniz. (${id})`);

          case 'ekle':
            var member = interaction.options.getMember('kullanıcı');
            var id = interaction.options.getString('mülk');
            var market = database.get('market') || {};
            var data = market[id];

            if (!member) return interaction.error('Kullanıcı bulunamadı.');
            if (!data) return interaction.error('Mülk bulunamadı.');

            await interaction.deferReply();
            var generatedId = interaction.client.properties.functions.generateId();
            await database.set(`mülkler.${member.user.id}`, (database.get(`mülkler.${member.user.id}`) || []).concat({ dataId: id, generatedId }));
            await member.roles.add(data.role).catch(() => null);
            return interaction.success(`${data.emoji} **${data.name}** isimli mülk **${member.user.username}** kullanıcısına eklendi. (${generatedId})`);
          
          case 'sil':
            var member = interaction.options.getMember('kullanıcı');
            var id = interaction.options.getString('mülk');
            var market = database.get('market') || {};
            var mülkler = (database.get(`mülkler.${interaction.user.id}`) || []).filter(value => market[value.dataId]);

            if (!member) return interaction.error('Kullanıcı bulunamadı.');
            if (!mülkler.find(value => value.generatedId === id)) return interaction.error('Mülk bulunamadı.');

            var data = market[mülkler.find(value => value.generatedId === id).dataId];

            await interaction.deferReply();
            await database.set(`mülkler.${member.user.id}`, (database.get(`mülkler.${member.user.id}`) || []).filter(value => value.generatedId !== id));
            await member.roles.remove(data.role).catch(() => null);
            return interaction.success(`${data.emoji} **${data.name}** isimli mülk **${member.user.username}** kullanıcısından silindi. (${id})`);
        };
    };
  }, 
  events: {
    autocomplete: async (interaction, { database, group, subcommand }) => {
      switch (group) {
        case 'market':
          switch (subcommand) {
            case 'sil':
            case 'düzenle':
              var market = database.get('market') || {};
              return interaction.client.properties.functions.autocomplete.respond(interaction, Object.entries(market).map(([id, data]) => ({ name: `${data.emoji} ${data.name}`, value: id })));
          };
        
        case null:
          switch (subcommand) {
            case 'ver':
              var market = database.get('market') || {};
              var mülkler = (database.get(`mülkler.${interaction.user.id}`) || []).filter(value => market[value.dataId]);
              return interaction.client.properties.functions.autocomplete.respond(interaction, mülkler.map(value => ({ name: `${market[value.dataId].emoji} ${market[value.dataId].name} | (ID: ${value.generatedId})`, value: value.generatedId })));
            
            case 'ekle':
              var market = database.get('market') || {};
              return interaction.client.properties.functions.autocomplete.respond(interaction, Object.entries(market).map(([id, data]) => ({ name: `${data.emoji} ${data.name}`, value: id })));

            case 'sil':
              var market = database.get('market') || {};
              var mülkler = (database.get(`mülkler.${interaction.options._hoistedOptions[0].value}`) || []).filter(value => market[value.dataId]);
              return interaction.client.properties.functions.autocomplete.respond(interaction, mülkler.map(value => ({ name: `${market[value.dataId].emoji} ${market[value.dataId].name} | (ID: ${value.generatedId})`, value: value.generatedId })));
          };
      };
    }
  }
};
