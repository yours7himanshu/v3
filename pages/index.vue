<template>
  <div>
    <LandingHero
      :header="$t('home.hero.header')"
      :subheader="$t('home.hero.subheader')"
    >
      <TPreview
        :content="$t('home.hero.bottom')"
        no-typo
        class="mt-2 text-xs text-center mx-4"
      />
    </LandingHero>
    <TCityHeader :profile="profile" view="festivals" global />

    <TFestivals :profile="profile" />
  </div>
</template>

<script>
import { db } from '~/plugins/firebase'
import { trackView } from '~/use/tracking'

export default {
  name: 'ExploreFestivals',
  async asyncData({ params, error }) {
    let profile = null
    let profileFound = false

    const collection = await db
      .collection('profiles')
      .where('username', '==', 'Travel')
      .get()

    if (collection.docs.length > 0) {
      const doc = collection.docs[0]

      profile = doc.data()
      profile.id = doc.id

      trackView('profiles', profile.id, profile.viewsCount || 0)

      profileFound = true
    }

    if (!profileFound) {
      error({ statusCode: 404 })
    }

    return {
      profile,
    }
  },
}
</script>
